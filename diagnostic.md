# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
    so that you know which tables are related
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
    Profiles
      id
      given_name
      surname
      email

    Movies
      id
      title
      release_date
      length

    Favorites
      id
      profile_id
      movie_id

    profiles can has_many favorite movies, favorite movies can has_many profiles
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
    has_many :movies, through: :favorites
    has_many :favorites, dependent: :destroy
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
    has_many :profiles, through: :favorites
    has_many :favorites, dependent: :destroy
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
    belongs_to :profile, inverse_of: :movies
    belongs_to :movie, inverse_of: :movies
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
    Serializers allow us to filter what is returned in a GET request
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :id, :movie
    has_many :movies
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
    bin/rails generate scaffold Favorites profile_id:integer movie_id:integer
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
    Dependent destroy is a method that will destroy dependents in a given table.
    We have used it to destroy the records in a join table related to records
    that we destroyed in another table.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
    For my project I was thinking of doing a ski list app, where one user has a
    series of many lists of items, but the many items can be on many lists.
  ```
