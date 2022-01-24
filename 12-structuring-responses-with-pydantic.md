# Structuring responses with Pydantic

* The Pydantic model to send back a response is created in _schemas.py_.

* You need to add an extra parameter called **response_model** in the decorator of the path operation to send back response in a schema you like (using Pydantic model).

    ```python
    @app.post("/posts", status_code=status.HTTP_201_CREATED, response_model=schemas.Post)

    def create_post(post: schemas.PostCreate, db: Session = Depends(get_db)):
        
        new_post = models.Post(**post.dict())
        db.commit()
        db.refresh(new_post)

        return new_post
    ```
    * Here, `schemas.Post` is the pydantic model we created in _schemas.py_ for sending back responses.
---
* And then you actually need to add a *Config* class in our Pydantic model. The reason why we are adding this is because we are returning an sqlalchemy model(**new_post**) which the pydantic model does not accept. It only accepts a dictionary by default. So we need to add this extra class to tell the Pydantic model to read the data even if it is not a dictionary. The FastAPI [documentation](https://fastapi.tiangolo.com/tutorial/sql-databases/) also talks about this.
    ```python
    # This is schemas.py
    from pydantic import BaseModel
    from datetime import datetime

    class PostBase(BaseModel):
        title: str
        content: str
        published: bool=True

    class PostCreate(PostBase):
        pass

    class Post(PostBase): # Response model
        id: int
        created_at: datetime
        class Config:
            orm_mode = True
    ```
---
* When returning the reponse from **create_posts**, we return a list of posts. So inorder to properly send it as a response model, we need to send it as a list.
    ```python
    from typing import List

    @app.get("/posts", response_model=List[schemas.Post])
    def get_posts(db: Session = Depends(get_db)):
        
        posts = db.query(models.Post).all()
        
        return posts

    ```
