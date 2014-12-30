# Spree Alchemy Example

A series of [Spree](https://github.com/spree/spree) + [Alchemy](https://github.com/AlchemyCMS/alchemy_cms) example applications. This branch (`master`) features the default installations of [Spree](https://github.com/spree/spree/tree/2-4-stable) (`2-4-stable`) and [Alchemy](https://github.com/AlchemyCMS/alchemy_cms) (`master`) as documented by their respective official documentation. There are custom patches to aid in the bridging of these two applications, which are denoted by a [`shoppuparty`](https://github.com/shoppuparty) (that’s me) fork or extension.

This application combines Spree with Alchemy and utilizes Spree’s default authentication [`spree_auth_devise`](https://github.com/spree/spree_auth_devise) to unify authentication. Spree and Alchemy engines are both mounted to root (`'/'`) which means frontend and backend route namespaces are combined. Meaning, if you create a `/products` route in Alchemy it will collide with the out-of-the-box Spree frontend `/products` route and Spree’s `/products` route with render.

However, this turns out to be not such a bad thing-- Spree and Alchemy do play well together in the way that the default route naming for the `/admin` namespace do not collide. With Alchemy mounted last, it acts as a catch-all for routes not defined by Spree. With that said, this is a Spree application enhanced by Alchemy. Eventually in your application you may want to consider phasing out Spree frontend entirely and have Alchemy handle all frontend routes. This of course will require additional bridges to be built between both applications, probably best as [Alchemy essences](http://guides.alchemy-cms.com/edge/essences.html).


# Application Versions

- Rails `4.1.8`
- Spree `2.4.2`
- Alchemy `3.1.0`

# Starting with this Project

I recommend creating this project from scratch using the steps [below](https://github.com/shoppuparty/spree-alchemy-example#project-creation). However, if you do want to use this project as a base for your project you will need to perform the following.

- update [`config/initializers/devise.rb`](https://github.com/shoppuparty/spree-alchemy-example/blob/master/config/initializers/devise.rb#L1) with a new `Devise.secret_key = "[SECRET_KEY]"`. 

```
# can use the rake command to generate one
$ rake secret
```

- [rename the project](http://stackoverflow.com/a/20990347/331759) from `spree-alchemy-example`.


# Project Creation

Here are the steps I used to create this branch if you do not want to clone this example and want to build yourself from the start. These steps are almost completely from Spree and Alchemy `README`s.

```
# install Rails required by Spree 2.4.2
$ gem install rails -v 4.1.8

# install Spree
$ gem install spree

# create Rails application with Spree 2.4.2’s required version of Rails
$ rails _4.1.8_ new spree-alchemy-example

# install Spree to Rails
$ spree install spree-alchemy-example
```

You will be prompted by Spree, this project and `spree_alchemy_spree_user` relies specifically on the `default authentication system` (`spree_auth_devise`) so it is important that is installed.

```
Would you like to install the default gateways? (Recommended) (yes/no) [yes] 
Would you like to install the default authentication system? (yes/no) [yes] 
Would you like to run the migrations? (yes/no) [yes] 
Would you like to load the seed data? (yes/no) [yes] 
Would you like to load the sample data? (yes/no) [yes] 
```

Devise will warn about a lack of a `Devise.secret_key`. Ignore it for now but grab the `Devise.secret_key = "[SECRET_KEY]"` line to be satisfied after Spree is setup.

```
[WARNING] You are not setting Devise.secret_key within your application!
You must set this in config/initializers/devise.rb. Here's an example:
 
Devise.secret_key = "[SECRET_KEY]"
```

Create the admin user that will be used for Spree and Alchemy.

```
Email [spree@example.com]: 
Password [spree123]: 
```

Create the Devise initializer from above with the `[SECRET_KEY]` provided. Obviously, do not use the actual value for this project.

[`config/initializers/devise.rb`](https://github.com/shoppuparty/spree-alchemy-example/blob/master/config/initializers/devise.rb#L1)


Add the Alchemy gems

[`Gemfile`](https://github.com/shoppuparty/spree-alchemy-example/blob/master/Gemfile#L46-L47)

```
$ bundle install
```

Create Alchemy initializer, this is required before installing Alchemy. A `Alchemy.user_class_name` needs to be defined.

[`config/initializers/alchemy.rb`](https://github.com/shoppuparty/spree-alchemy-example/blob/master/config/initializers/alchemy.rb#L1-L4)

```
$ rake alchemy:install
```

Alchemy will create some files and copy over migrations. There will be a conflict with `app/views/layouts/application.html.erb`, just enter `n` to proceed through to the installation. Mount Alchemy to root as well (if prompted), both of these files will be changed in the next steps.

Replace your layout view.

[`app/views/layouts/application.html.erb`](https://github.com/shoppuparty/spree-alchemy-example/blob/master/app/views/layouts/application.html.erb#L1-L16)

Replace your routes file to define Alchemy to handle routing of root and non-Spree routes. Additionally, define a Devise route for `spree_auth_devise` to match the `logout` request method used by Alchemy.

[`config/routes.rb`](https://github.com/shoppuparty/spree-alchemy-example/blob/master/config/routes.rb#L1-L13)

```
# start your webserver to verify installations
$ rails s
```

Navigate to `http://localhost:3000` in your web-browser. You should arrive at a Rails error page, after Alchemy redirects you to `/index`. This is normal, Alchemy does not have an `index` to render since it is a fresh install. To fix this, navigate to `http://localhost:3000/admin` and you should be in Spree’s login view. Login with the admin credentials from the Spree installation and you should arrive at the Spree Orders page.

The `spree_alchemy_spree_user` gem creates a link "Alchemy" in the Spree Admin menu, click that to jump into Alchemy’s Admin and once there, create the `index` page for your default language and publish that page, then `logout` to see the Alchemy rendered `http://localhost:3000/index`.

You probably do not want to commit the product images created by the Spree seed, among other things.

[`.gitignore`](https://github.com/shoppuparty/spree-alchemy-example/blob/master/.gitignore#L18-L21)
