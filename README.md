# SQLAlchemy Cheat Sheet

SQLAlchemy is a powerful SQL toolkit and Object-Relational Mapping (ORM) library for Python. It provides a full suite of well-known enterprise-level persistence patterns, designed for efficient and high-performing database access. This cheat sheet provides a quick reference for common SQLAlchemy commands and usage patterns.

## Installation

To install SQLAlchemy, you can use pip:

```bash
pip install SQLAlchemy
```

## Getting Started

1. **Import SQLAlchemy:**

    ```python
    from sqlalchemy import create_engine, Column, Integer, String, ForeignKey
    from sqlalchemy.ext.declarative import declarative_base
    from sqlalchemy.orm import relationship, sessionmaker
    ```

2. **Create an Engine:**

    ```python
    engine = create_engine('database://user:password@host:port/dbname')
    ```

3. **Create a Session:**

    ```python
    Session = sessionmaker(bind=engine)
    session = Session()
    ```

4. **Declare a Base:**

    ```python
    Base = declarative_base()
    ```

## Defining Models

```python
class User(Base):
    __tablename__ = 'users'
    
    id = Column(Integer, primary_key=True)
    name = Column(String)
    
    addresses = relationship("Address", back_populates="user")

class Address(Base):
    __tablename__ = 'addresses'
    
    id = Column(Integer, primary_key=True)
    email = Column(String)
    user_id = Column(Integer, ForeignKey('users.id'))
    
    user = relationship("User", back_populates="addresses")
```

## Basic Queries

- **Query all Users:**

    ```python
    users = session.query(User).all()
    ```

- **Filtering:**

    ```python
    user = session.query(User).filter_by(name='John').first()
    ```

- **Joining Tables:**

    ```python
    query = session.query(User).join(Address).filter(Address.email == 'john@example.com')
    ```

## Adding, Updating, and Deleting

- **Adding a User:**

    ```python
    new_user = User(name='John')
    session.add(new_user)
    session.commit()
    ```

- **Updating a User:**

    ```python
    user.name = 'New Name'
    session.commit()
    ```

- **Deleting a User:**

    ```python
    session.delete(user)
    session.commit()
    ```

## Advanced Queries

- **Grouping and Aggregating:**

    ```python
    from sqlalchemy import func
    
    count = session.query(func.count(User.id)).scalar()
    ```

- **Subqueries:**

    ```python
    subquery = session.query(func.max(User.id)).scalar_subquery()
    user = session.query(User).filter(User.id == subquery).one()
    ```

## Resources

- [SQLAlchemy Documentation](https://docs.sqlalchemy.org/en/20/)
- [SQLAlchemy Tutorial](https://docs.sqlalchemy.org/en/20/orm/tutorial.html)

## License

This cheat sheet is provided under the MIT License. See the LICENSE file for details.
