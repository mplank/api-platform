# Set Uuid

Coming soon...

Okay. The U UID is now the identifier inside of our user resource. Awesome. But it's
still working exactly like the old ID. What I mean is only the server can set the
UID. Alright. If we tried to send you your ID as a field, when we recur, when we're
creating an user, it would not work. How do I know? Because if you look at our user
class, you ID is not settable anywhere. It's not an argument that constructor and
there's no set UID fields. So I know that it's definitely not going to be settable in
our API, but I do want to make that possible. Let's describe this in a test first. So
in user resource test, I'm going to go up to the top and copy test, get user.

Is that down here? And we'll call it, test, create user with you UID. And we'll just
kind of simplify this a little bit down here for the JSON. And in addition to the
other fields, I'm not going to pass you. You ID = and for you, your ID let's actually
set one up here. So I'll say you UID = U U I D. You're the one from Ramsey. You your
ID four. And then down here, I will send that you, you ID field and I technically
could call to string on it, but it has an underscoring as two string. So don't
actually have to do that then, or assert a certain service code is two Oh one and
down here, I will take out these, uh, profession, user object stuff. Cause I can just
basically assert the ad ID is going to be /API /users /and then that you, you ID and
I'll also take out the login part, make sense.

We send to you your ID. It should use that your, your ID. So I'll talk top copy of
this method name here and let's spin over and run PHP Symfony PHP bin /PHP, unit dash
dash filter = task to create user with you, you ID and perfect. It doesn't work. It's
just completely ignoring the UID that we're sending and then choosing a different
one. Okay. So how, how can we make the UID a field that is settable? Well, it's
really no different than any other field of we're going to make it settable. We need
to put it inside of the correct group and make sure that it's set, make sure that
it's set of all either through the constructor or via setter method. Now we could add
a setter like a set UID, but then we would need to be careful to add the correct
groups to it.

So that it's writeable when we create a user, but not modifiable later, we don't want
you, you ideas to change after the creative or we can do something that's a little
more, a little bit more natural. We could avoid the setter, but then add it as an
argument to our constructor. If it's only available as an argument to an instructor,
then naturally it's not going to be something that's going to be updatable because
there's simply no way to update it. So let's do that. I'll add a U U I D interface
your UID argument. And I'll set that to know. And then down here, I'll say this area,
you had the = and I'll say you UID, colon question, Mark Colon, and then your UID
for, so if it has passed, we'll use that. If it's not passed, then we'll generate a
random one. And then we need to make sure that the UID is actually part as rideable
in our API. So up here on our, you, your ID field, I will add at groups and inside of
here, I'm going to say user colon, right? So it is writeable. But of course, if it's
not going to be edited, you know, you can only send it when we're creating an object.
You can't change it afterwards.

All right. So now if we run the test, it passes, it works. That was awesome. And the
documentation for this instantly looks perfect. Let's try, I'll refresh the API
homepage. Then open up the post operation for users hit, tried out and you can see
it's already kind of giving me a UID example here understands that this is a
[inaudible] and it is available at for us to set, but wait a second. How did that
work? Because if you think about it, if you look at our test, we're sending a
[inaudible] string, of course, but ultimately on our user object, the first argument,
the constructor is some sort of you, you identity, you, you ID interface, object, not
a string.

Remember API platform. Well really Symfonys. See serializer system is really good at
reading your types. So in notice that the type for you ID is a ID interface, and then
it found, try to find a DC realized, or that knows how to DC realize fields into you.
UID interface objects in API plat platform comes with a DC realizer for Ramsey. You
UIDs out of the box. That is why that worked by the way, there is one little quirk
with you UIDs and that is they can't be called ID. Let me show you what I mean head
up to the year. You're already a property and let's pretend that we just want to call
this ID, uh, in the, uh, API. So for example, instead of sending you a field called,
uh, your ID here, I just want that to be called ID. So to make that happen, I can add
an at serialized name here ID, and then over my test, it simply means that I just
need to change that field, right? We've done this type of thing before. Normally that
works fine, but if we rerun that test, it fails and it says an Erik or an update is
not allowed for this operation. So this is basically a bog /complex area inside of a
API platform where in the JSON, and right now there's not a fix for it. There is a
workaround. However, whenever you send the data, the APM platform, it tries to figure
out what format you're sending it.

You know, for example, if we send XML, we would need to tell it, uh, it would need to
know that that's XML. So we can DC to realize it and back smelt by default and assume
that you're sending JSON, however, in the same way that you can get content back as,
uh, as JSON or JSON LD. So for example, by default, we get back JSON LD format. So
when we use an API and point blank /APSs Jesus, we can get things back as JSON L D,
or we can actually get things back as JSON. So there's when you request a resource,
you can tell it what outputs you want. You can actually do the same thing. When you
send an input, you can actually send it data and say, the data I'm providing you is
JSON or XML. Now it doesn't make much difference on the input format, but you can
actually tell API platform that I'm sending you JSON L D format of data versus JSON.

And if you do that, that actually makes just a small enough difference in how the,
how the content is DC realized that it avoids this air. There's a long way of saying
that the workaround ground for this is actually descend a content type header. So in
the test, I'll set headers and I'll set that and I'll send that to an array with
content type set to application /LD. Plus JSON, we're trying to test out it passes
now, to be honest with you, I don't really know if that makes any other changes in
how our objects do seem to realize it doesn't seem to, um, but full disclosure. I
don't know that for sure it does around this bug. So the destabilization logic is
slightly different internally when you're passing this, now this work ground works
great. If you are the only person using your own API, because you just need to know
to include this header. But if other people are going to be using your API, then it's
kind of ugly for them to have to pass this content type header to avoid that error.
So another workaround is actually to disable the JSON format entirely in always
forced JSON LD.

So I'm actually gonna do is remove this header here, and then I'm going to go into
config packages. APN is core platform, dynamo, and down here, I'm going to comment
out the JSON format. Now, as soon as I do that, whenever we send JSON input, it's
going to use JSON LD is the default format instead of JSON. There's one other spot I
need to change just so I don't get an error. If I go into entity, she's listing, uh,
an uh, past tutorial, we actually, uh, set specific formats for one of our
operations. No, was that specific format for this? I'm actually going to remove JSON
from the list and since no longer even available as a global format. So now I'm not
passing the header anymore and it still fails.

Oh, of course, huh? Because forgot. I need to actually keep this line right here.
There we go. So this tells it, if we send data applications, that's JSON use this
type course. That's the piece that I was missing. It's now it's going to interpret
that as using the JSON LD format when we sign it and now I pass this. So I just want
to be aware of that limitation and that workaround. I'm actually going to remove all
of this and, uh, and be content with using just you UID. So I'll put that format
back. I will re add it to my JSON and then over inside my resource, I'm actually
going to remove that serialized name so we can get back to the good spot.

Okay. That's it. Whoa. We just like API platform and really rest in general to
another level of flexibility with custom fields, custom API resources, and a lot more
in the next course, we'll dive a bit deeper into one last area we haven't talked
about yet, which is true custom end points, uh, including custom end points that are
done in a controller and also custom end points that are done with messenger
integration and Symfony. And if there is something that we still haven't talked about
yet, that you really want to know about, let us know in the comments. Okay. Go make
some awesome JSON, and we'll see you next time.

