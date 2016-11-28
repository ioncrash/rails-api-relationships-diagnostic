# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

```sh
  if you want to have a many-to-many relationship. For example, ingredients appear in many different recipes, and recipes have many different ingredients. Since they're on separate tables, you need a join table
```

1.  Provide a database table structure and explain the Entity Relationship that
describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
(Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
`Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
join table with references to `Movies` and `Profiles`.

```sh
  Profile columns: id, given_name, surname, email
  Movies: id, title, release date, length
  Favorites: id, profile_id, movie_id
```

1.  For the above example, what needs to be added to the Model files?

```rb
class Profile < ActiveRecord::Base
  has_many :movies, through: :favorites
  has_many :favorites
end
```

```rb
class Movie < ActiveRecord::Base
has_many :profiles, through: :favorites
has_many :favorites
end
```

```rb
class Favorite < ActiveRecord::Base
belongs_to :movie, inverse_of :favorites
belongs_to :profile, inverse_of :favorites
end
```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

```sh
  a serializer controls what columns a user can access.

class FavoritesSerializer < ActiveModel::Serializer
  attributes :id
  has_one :profile
  has_one :movie
end

```

```rb
class ProfileSerializer < ActiveModel::Serializer
end
```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

```sh
  rails g scaffold favorite movie:references profile:references
```

1.  What is `Dependent: Destroy` and where/why would we use it?

```sh
  Dependent destroy ensures that if you delete a member of a table, all of its dependents on the join table are deleted too. Ensures that you don't have orphans in your join table
```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

```sh
  Maybe you have a salesperson assigned to many clients (one-to-many). You also have many sales packages assigned to many clients. So a client can have many sales packages, which can be sold to many different clients, but each client only has one salesperson
```
