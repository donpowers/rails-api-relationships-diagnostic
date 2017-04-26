# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
  It prevents redundant data.  This data would have to be maintained
  it several tables and would easily become unmanageable why a change is needed.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
  Profile     Movies
  given_name  title
  surname     release_date
  email       length
  profile_id  movie_id

  Favorites
  profile_id
  movie_id

  In this case a user may have a 'wish list of movies' we could store. We could have
  many users who have many favorites, hence the many to many relationship.
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
  has_many :favorites
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
  has_many :favorites
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
  belongs_to :profile, inverse_of: :favorite
  belongs_to :movies, inverse_of: :favorite
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
The role of the serializer controls the data that is passed back from the
query.  The columns(data) from the table are listed here that are to be returned.
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
  attributes :profile_id, :given_name, :surname, :email
end
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
script/generate scaffold movies profiles category:references
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
Dependent: Destroy is used when deleting data in the join table.  It ensures
all the dependent tables are updated when that data is deleted.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
A book typically belongs to one author, there are cases that where there maybe
more then one author. 
  ```
