# JWT Tokens

A **JWT Token** is something that our API server sends back to the user, once the user is authenticated, ie- once the credentials provided by the user matches the credentials stored in our database. This is one type of authentication, a token based authentication.
* This token is not encrypted.
* The token is made up of 3 different pieces - Header, Payload and Signature. 
* The **header** includes metadata of the token. It mostly contains the algorithm we use for signing our token and the type of the token (JWT in our case).
* **Payload** is nothing but data that we wish to send back. But we need to keep in mind that, if we jam a lot of information here, it is going to increase the size of our packet.
* The **signature** is a combination of three things.
    * The header
    * The payload
    * And a secret key or a special password, whatever you wanna call it but it's private and only stored on our API server.
    * We take these three things and pass it into our signing algorithm, which is going to return a signature.
* Now, when the user tries to access information that requires authentication, the token from the user is sent to the API. 
    * Now, the signature verifcation process happens. ie- the header and the payload from the token that the user sent is extracted and passed in to the signature algorithm along with the secret password that is stored only on our API server. 
    * So, the signature returned from the algorithm, will be the actual signature which is verified with the signature present in the token sent by the user. If it matches, then the user is provided access, else not!

## OAuth2PasswordRequestForm

Until now, we were receiving the user credentials and validating them in the form of the pydantic model. But when dealing with authentication, we can receive it using a built-in utility in the *FastAPI* library. ie- in the form of OAuth2PasswordRequestForm. 
* This object contains two fields, username and password. So even if we send an email, it is going to be stored in the field 'username' which is just the name of the field.
* And one more change is that, when we use OAuth2PasswordRequestForm, we can no longer send data in raw form in postman, instead we need to send it as form-data as keyvalue pairs.
    ```py
    from fastapi import APIRouter, Depends, HTTPException, status
    from fastapi.security.oauth2 import OAuth2PasswordRequestForm
    from .. import database, schemas, models, utils, oauth2
    from sqlalchemy.orm import Session

    router = APIRouter(tags=['Authentication'])

    @router.post('/login', response_model=schemas.Token)
    def login_user(user_credentials: OAuth2PasswordRequestForm=Depends(), db: Session = Depends(database.get_db)):

        user = db.query(models.User).filter(models.User.email == user_credentials.username).first() # The email is actually stored in the 'username' field of the OAuth2PasswordRequestForm

        if not user:
            raise HTTPException(status_code=status.HTTP_403_FORBIDDEN, detail="Invalid Credentials!")
        
        if not utils.verify(user_credentials.password, user.password):
            raise HTTPException(status_code=status.HTTP_403_FORBIDDEN, detail="Invalid Credentials!")
        
        token = oauth2.create_access_token(data={"user_id": user.id, "message": "Asif da boss!"})

        return {"token": token, "token_type":"bearer"}
    ```

## Creating jwt tokens

The _FastAPI_ [documentation](https://fastapi.tiangolo.com/tutorial/security/oauth2-jwt/) covers how to create jwt tokens and use them.
```python
from jose import JWTError, jwt
from datetime import datetime, timedelta

SECRET_KEY = "09d25e094faa6ca2556c818166b7a9563b93f7099f6f0f4caa6cf63b88e8d3e7"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30 # Decides how long the user should be logged in 

def create_access_token(data: dict):
    to_encode = data.copy()
    expire = datetime.utcnow() + timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES) # time that our token is going to expire
    to_encode.update({"exp": expire})
    encoded_jwt = jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
    
    return encoded_jwt
```
* We deal with jwt tokens in a new file called _oauth2.py_. 
* And now in _auth.py_ where the login operation takes place, we call this function to create a new token
    ```python
    access_token = oauth2.create_access_token(data={"user_id": user.id, "message": "Sanjeev da boss!"})

    return {"token": access_token, "token_type":"bearer"}
    ```
    * As we saw walier, we are allowed to pass in anything in the payload. So here, we simply pass in the user_id and a message.


## Verifying jwt tokens
Once we've created the jwt token, we need to create a function for verifying the tokens aswell.
* We need to check if the token has not been tampered and contains the data we actually expect
* We also need to make sure that the token hasn't expired.
```python
oauth2_scheme = OAuth2PasswordBearer(tokenUrl="login") #tokenUrl is the name of the route
# oauth2_scheme basically extracts the token from the request
def verify_access_token(token: str, credentials_exception):

    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM]) # Decodes the data from payload

        id: str = payload.get('user_id')
        
        if not id:
            raise credentials_exception
        
        token_data = schemas.TokenData(id=id) # Validates the token data with the Pydantic model asuuring we get all the fileds with the specif datatypes we expect

    except JWTError:
        raise credentials_exception

    return token_data

def get_current_user(token: str = Depends(oauth2_scheme), db: Session = Depends(get_db)):
    credentials_exception = HTTPException(
        status_code=status.HTTP_401_UNAUTHORIZED,
        detail="Could not validate credentials",
        headers={"WWW-Authenticate": "Bearer"}
        )
    
    token = verify_access_token(token, credentials_exception)

    user = db.query(models.User).filter(models.User.id == token.id).first()

    return user
```
* The *get_current_user* function returns the user from the database with the user id extracted from the payload of the token.

## Adding the dependency in the path operation
For every operation that needs the user to be logged in, we create a dependency function in that specific path operation which calls the *get_current_user* function which inturn verifes the token and returns the user.

```py
@router.get("/{id}", response_model=schemas.Post)
def get_post(id: int, db: Session = Depends(get_db), current_user = Depends(oauth2.get_current_user)):
```
* And now for every function with the dependency we need to pass in the token with our request. To do that, we go to the headers section in postman, and add the data
```
Key: Authorization
Value: Bearer <actual_token>
```
* Or we can also go to the Authorization tab, select the type of the token (Bearer token) and provide the token.