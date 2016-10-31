# ManyToMany & Fixtures

Head back to `/genus`. These genuses are coming from our fixtures, but, sadly, the
fixtures don't relate any scientists to them... yet. Let's fix that!

The `fixtures.yml` creates some `Genus` objects and some `User` objects, but nothing
links them together. How can we do that? Well, remember, the fixtures system is very
simple: it simply sets each value on the give property. It also has a super power
where you can use the `@` syntax to reference another object. In that case, that
other *object* is set on the property.

Setting data on our `ManyToMany` is no different: we need to take a `Genus` object
and set an *array* of `User` objects on the `genusScientist` property. In other words,
add a key called `genusScientists` set to `[]` - the array syntax in YAML. Inside,
use `@user.aquanaut_1`. That refers to one of our `User` objects below. And whoops,
make sure that's `@user.aquanaut_1`. Let's add another: `@user.aquanaut_5`.

It's not very random... but let's try it! Find your terminal and run:

```bash
php bin/console doctrine:fixtures:load
```

Ok, check out the `/genus` page. Now *every* genus is related to the same two users.

## Smart Fixtures: Using the Adder!

## Randomizing the Users

But wait... that should *not* have worked. The `genusScientists` property - like
*all* of these properties is *private*. To set them, the fixtures library uses
the setter methods. But, um, we don't have a `setGenusScientist` method, we only
have `addGenusScientist()`.

So that's just another reason why the Alice fixtures library *rocks*: it figured
this out automatically:

> Hey! I see an `addGenusScientist()` method! I'll just call that twice instead of
> looking for a setter.

## Randomizing the Users

The only way this could be more hipster is if we could make these users random. Ah,
but Alice has a trick for that too! Clear out the array syntax and instead, in quotes,
say `3x@user.aquanaut_*`.

Check out that wonderful Alice syntax! It says: I want you to go find *three* random
users, put them into an array, and *then* try to set them.

Refresh those fixtures!

```bash
php bin/console doctrine:fixtures:load
```

Then head over to your browser and refresh. Yep, three random scientists for each
Genus. Pretty classy Alice, pretty classy.