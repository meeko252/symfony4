# Downloading Private Files

Coming soon...

We are successfully uploading article references and they are going into this Var
uploads article referenced directory. That's great. Uh, we need these, we did this,
we moved them here instead of the public directory because we need to check security
before somebody can download them. So the question is how can we make these
downloadable if they're outside of the documentary? Well, let's find out. Let's start
inside of our edit template, but I want to do first is actually list all the
references that we've saved in the database. So an edit that instrument twig and you
out awesome bootstrap classes and they'll say for reference in and we can use article
dot.

Okay.

Article references to get all of the article references that we've uploaded so far.

Okay.

Then we'll add a u l I put a bunch of classes on here just to style things nicey what
we're actually going to print out is a reference, not a rich [inaudible] file name.
This is the coolest thing, right? When we moved down to the server, we gave it a
weird file name or we saved the original file name here so we can actually show that
to the user. They don't even know that we have actually given it to a different file
name, uh, internally.

So when you're fresh that cool. There we go. We've got our two uploaded references.
So how can we turn this into a download link? Well, let's start with the end. Wait,
this is going to work is we can't just link the user directly to the file. We're
going to need to link them to a Symfony route and controller and that route and
controller is going to check security and return the file to the user. So we'll do
this an article reference admin control it. This is basically a new end point for
downloading. So public function. How about download article reference won't put the
APP route above this with Admin admin. /article /references. /id. So nice clean URL
to get that download that one specific reference.

Okay.

Don't put /download on the end of the URL and then name equals. And we use admin
article, download reference and methods = get, and I don't need to do this, but that
part's optional but it's nice. So not, let me have the ID of the article preference
in the you were out. We can type hint article reference and that's just Dee Dee
reference to make sure that that is working correctly. Cool. Copy that route name.
And now we can just go indoor our edit template and make a link to that. I'll put a
span here that's just freestyling a draft path. Pace that name.

Yeah,

I move the go on to multiple lines here. Pass Id said to reference.id.

Okay.

And on the anchor we'll just put a little fun isom font. Awesome. Icon FAF a dash
download.

Cool.

Now going to refresh and one law or link to our article reference download. All
right, so now our job in here, it's just to go get the contents of that file in, send
them back to the user. But what we don't want to do is just get all of the content of
the file and a string and then return. It's because if you are a, if the user is
downloading a large file and if you read that entire file into memory, it's going to
be your PHP member. You could actually run out of memory. So once again, in the same
way that in our upload or helper we wrote to the stream up to when we saved it, we
are going to read a stream and return that stream back to the user. So I'm going to
start with here is a new public function inside of our upload or helper that's going
to allow us to um, get a stream for any of our uploaded files. And we're gonna use
fly system for this because again that file might live on s three. It might live down
on our, on our server. So public function and it looked like this.

Oh functioning for read stream. We'll pass it the path to the file. So cause
something like article reference last Stephanie Best Practices. And then we also need
to have a boolean for his public. Don't remember. We right now we actually have two
different filesystems, um, inside of our project, a public filesystem and a private
filesystem. So we need to know which filesystem we're going to read this file from.
And I'm going to advertise that this is going to return a resource. There's no return
type right now for resource and PHP. Our first thing, we'll get the right filesystem.
So if it's public, we're going to read where you use this->filesystem.

It's private, we use the private filesystem and then

pretty much every method for reading, writing, updating on fly system has a way to do
it as GE, as a string or a stream. So we can say filesystem,->read, or we can say a
filesystem, Aris Stream, which is what we want and I'll pass that the path and then
remember all of the methods in, um, why system if they fail to return false. So I
want to code defensively here. So if false, if resource is false, we'll throw a new
exception with a nice message saying air opening stream 4% s and we'll pass that
path. That's the bottom, just return resource. So now I have a really nice way of
reading a resource for any file in any of our fly system filesystems.

Okay.

So back in our controller, we're going to need to get the uploader helper in this
method now. So I'll add an uploader helper arguments. And actually before we use
that, um, we talked about security. Actually this, there's no security on this end
point yet. I actually forgot to add that. So normally the way we do security with our
article admin stuff is that, um, we're, we're using, this is granted manage article
that's using a custom voter and make sure that you are either a super admin or you
are the author of this article. We can use the annotation here because we actually
have an article argument. In this case, we don't have an article arguing and article
reference argument. So I want to do a kind of a little bit more of a manual way. I'm
gonna say Article = Reference->get article and then I can say this->deny access and
less granted you can pass it that same manage key and the article. So that does the
same thing as the annotation. I forgot refresh to make sure that works. Yeah, we
still have access because we are logged in as an admin.

All right, so how can we return a stream to the user? You know we're used to saying
like, you know, returning us response object or a JSON Response Object. If you
actually want to stream something to the user, you need to use a special class called
a streamed response. So we can say response = a new streamed at response and this
actually takes a call back as an argument.

The idea here is that it's bottom, we'll return response India here is that when
Symfony is finally ready to, we can't just start streaming the response right here in
the middle of our request handling. Um, we need a, or a stringer responsible on
Symfony is completely done with everything that it needs to do. So the data here is
when he returned a stream response, Symfony stores that object and then when it's
finally ready to send the content to the user, it caused our function. And we can do
whatever we want here. We can actually echo food we want it to and that would come up
to the user. So in our case, we want to get a stream and send that to the user. So
I'm actually gonna add a use here because we need to get the reference variable in
here and also the uploader helper. You need to do that so that we can use those
variables within this function. Versus I have to do is to set up an output stream.
This might look a little bit strange. PHP Colon, /slash. Output

and then WB is an argument. And then we're going to read the actual file stream. So
we're going to say upload or helper, aero read stream. And here we're going to pass
the path to our files. So Article Reference Lasts Symfony Best Practices, um, but we
actually don't have an easy way to do that yet. You might remember in our article
entity we have and I spent the night here called get imaged path, which uses this
constant from article upload a helper and then actually concatenates the file name.
Let's actually copy that. We're going to do the exact same thing inside of article
reference, so anywhere inside of here we'll make a new function. Let's call it.

Okay,

get file path.

Okay.

Add a return type or redirect the R and upload or helper to get the use statement
changes to article reference and then change this to get file name so we don't have a
really nice way to just get the file path for any reference. So back in our
controller, we need to pass that path here. I can now say reference Arrow, get file
path and then we need to return false for the is public, so it uses the private to
filesystem.

Okay,

finally see this output stream here, this is the strange little thing. The piece be
colon /SAS output is basically the way to open a stream that if you send thing, if
you write something that stream, it's basically the same as echoing content. It's
going to send it to the output. So we basically want to take the file stream and send
it to the output stream. And to do that you can use,

yeah,

a peach tree function called stream copy, the stream and your copy, the file stream
to the output stream. Again, this is basically a fancy way to send that file to the
user without ever actually reading that file into memory. And yeah, that should be
it. So let's give that a try and move over, refresh and got it sort of, I'm going to
hit us. Wow. Okay. So clearly that is sending us the content, but our browser
browsers not handling it. And the reason that we haven't told the browser what type
of file this is, we haven't told them, hey, we're sending you a pdf file and you
should download it or anything like that. So the first thing that we need to make
sure we do is we should actually tell the browser what type of file this is. Um, if
you can, and fortunately when we uploaded this file, we actually store it, the mime
type. We're just kind of a nice, because, um, Pete and PHP, you do have the ability
to read the my type from a file. But if this is stored on cloud storage to get read
the mind type, you might need to download it and then read the mime type. So it's a
nice thing to store if you need it. So back in our controller we can say response,
Aero headers,->set, content dash type. We'll say reference Arrow, get my type,

I if you move over and refresh hello pdf. And the one other thing you might want to
do and it's up to you, is um, you notice this open in my browser. If you want to, you
can actually force a download so that things don't show on the browser, but they
actually download and wait. You do that as you need to set this hetero called content
dash disposition. And it's kind of a weird header to create. So Symfony has a helper
for it. So check this out. We'll say disposition = and then we're gonna use this
class called header and utils colon colon making disposition. So the idea here is
that we're first going to tell it, do we want, we're going to pass it a disposition
stream, which is basically whether or not we want the user to download it or open it
in their browser.

So we can use one of these two constants here, had utils attachment or inline, we
want attachment. And then we're going to give it the actual file that we want, which
is really cool because remember we don't want to just send them like we don't set the
file name, they tried to download it. It's probably gonna download to like with the
name download cause that's what it looks like in the URL. So in here we can actually
use the original file name, reference Arrow, get original file name, some of the
download the file to actually download it with the correct, uh, file name. So let's
DD disposition, move over, refresh. And there it is. So this is just a string. This
is how the header needs to look if you want to do this type of thing. It's just kind
of a weird thing. Weird strings. That's why there's a helper for it. Ultimately, we
want to say response, Arrow, headers,->set, content type. How did that disposition
and pass the disposition to that? And now when I dry it, boom, downloads instead of
opens up in our browser.