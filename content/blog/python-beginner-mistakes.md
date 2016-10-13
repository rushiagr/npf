+++
Categories = ["Development", "python"]
Description = ""
Tags = ["Development", "python", "vim", "git", "programming", "django"]
date = "2016-10-11T17:59:46+05:30"
type = "post"
title = "Python Beginner Mistakes"

+++

I am writing Python code since 5 years, and I have worked with around 5-6
entry-level Python programmers. There is a pattern I can see in the programming
practices of such programmers, so I am listing the problems and their solutions
in this article. I have worked with OpenStack for over three years, which has a
very high code quality, so I feel I am qualified enough for writing this post.
My hope is this will be helpful for more entry-level Python programmers.

I'll start with talking about these beginner-programmers first. The people I
have worked with are not only self-taught Python programmers, but who also
worked in startups in Bangalore. For self-taught Python programmers, it is
understandable -- Python is a pretty easy language to pick up and code, so
there is really no motivation for them to go with best practices right from the
start, especially when all they can find is a huge list of good programming
practices and PEP8 conventions to follow. For Python beginners working in
startups, I am guessing there are not enough senior Python programmers who can
teach them about required best practices.

In this article I'm going to list the bare-minimum of these simple practices
you should follow. Surprisingly, most of the 'Beginner Python mistakes to
avoid' articles on the web don't talk about the bad practices I encounter. I
think they just assume such simple things are taken care of by a programmer,
my experience tells otherwise.

Note that this is just the beginning. There's a TON of things you should worry
about, but you'll learn them as you grow. Don't worry :)

## Indentation, tabs and spaces.
Never use tabs. Only use spaces. Use 4 spaces for a tab, always.


I frequently see codes which use a mix of spaces and tabs, and 2-space and
4-space indentations all in one file!  If you are coming from a different
language, or if you already have a notion of, say, 8 spaces for a tab, then it
might be difficult and seem unnecessary at first. But I am guessing you are
liking the language and you look forward to writing lots and lots of Python
code in your lifespan; in such a case, just bear with me for a few weeks and
it'll come naturally to you, trust me :)


## Raise specific exceptions

Don't write like this:

    try:
        # do something here
    except Exception as e:
        # handle exception

Catching the parent `Exception` is bad. If you know what exception the code is
going to throw, just except that exception. The most common example I have seen
is when calling `get()` method on a Django's model. If the object is not
present, Django throws `ObjectDoesNotExists` exception. Catch _this_ exception,
not Exception.

Most often, you do some corrective action in the 'handling exception' part. If
you are excepting (if there's a word 'excepting') `Exception`, then you are
heading straight into this 'handling exception' section no matter what has
happened. Your database might be down but the 'database down' exception might
still be caught and probably silently ignored of you do this.

## Trailing whitespaces
In context of Python, whitespaces means space characters. There should be no
line in Python code which has spaces at it's end.

You might think why make such a big issue of this. The problem is
version/source control. You invariably will use a source control system at some
point of time, for example 'git'. When you commit trailing spaces, it becomes
difficult to read the diffs (the difference between two commits/checkins).
Also, removing or adding more spaces might feel okay if you develop a habit of
leaving around trailing spaces. But when you will start using a source control
system, this will add unnecessary noise in commits, making code review and
debugging difficult. Remember, code is read much times than is written or
modified.

## Edge cases first. Shallow nesting.

Consider two pseudo codes. These are codes for handling a request, but you can
altogether ignore that fact and it would still make sense:

Pseudo code 1:

    if request is post:
        if parameter 'param1' is specified:
            if the user can be authenticated:
                if the user can perform this action:
                    # Execute core business logic here
                else:
                    return saying 'user cannot perform this action'
        else:
            return saying 'parameter param1 is mandatory'
    else:
        return saying 'any other method apart from POST method is disallowed'


Pseudo code 2:

    if request is not post:
        return saying 'any other method apart from POST method is disallowed'

    if parameter 'param1' is specified:
        return saying 'parameter param1 is mandatory'

    if the user can be authenticated:
        return saying 'user cannot be authenticated'

    if the user can perform this action:
        return saying 'user cannot perform this action'

    # Execute core business logic here

As you can see, the second code is much easier to read. You will also notice
that the second code doesn't have a lot of nesting, which avoids common
problems. Can you see that I have not written an 'else' section? If the core
logic is several dozens of lines long, it becomes difficult to find out which
`else` belongs to which `if` if we write in 'Pseudo code 1' style.

I am sure while starting to write, pseudo code 1 should feel normal. But with
very minimal effort you can very soon get used to writing code in 'pseudo code
2' style.


The following are minor, probably opinionated suggestions. These might apply to
not only Python but any programming language. I feel that these points are
important enough that you should start a habit of adhering to them right from
the start. It'll help you in the long run, trust me :)

## Make commit messages lengthy
This is a fairly general trend I see. The commit messages such people generally
write are just a few words long. Most of the beginners have started using `git`
as version control system. In git, such people have gotten used to using `git
commit -m "your commit message here"` command. If you use this command, I am
sure you won't even feel like writing a commit message more than what fits in
one line :). Do you know that you can just type `git commit` and it will open
up an editor (vim or nano) to write the commit message? Start using this way,
and stop using `-m` flag directly on command line. It'll help you and your
colleagues :).

I generally prefer to write a short description in the first line of commit
message, then leave a blank line, and then write a paragraph after that to
explain in more detail if required. The first line is the most important. Try
to describe as much as possible concisely in this single line. But don't ever
let this line go above 100 characters, preferably 80. See this commit message
as an example:

    Don't allow access to POST /v1/purchases API without authentication

    It was revealed during testing that the above mentioned API is accessible
    even when accessed without authentication. This is a security risk. This
    commit fixes it. One 'if' condition added to reject API call if the user is
    not authenticated.

You might feel that it's a lot of effort to write such log git commit messages.
But you can realise it'll not take more than 2 minutes to write this. Much less
than how much time you spent writing code corresponding to this commit :)

Note that commit messages are important part of documentation, and are
extremely important for debugging later.

## Do only one thing in a commit
If you are fixing three issues, make sure you create three different commits
for each one, and never one big commit to include three bug fixes. Even if each
of these bugfixes are just a couple of lines. If you are doing a feature work,
then it's okay to put all the feature work in one single commit, but for a
bugfixes, create a separate commit for each one. As you can see, you should get
comfortable with the idea that a commit can be as short as one line and as
large as couple of hundred of lines.  Let go of the feeling that your brain has
about lines in a commit, that it feels right only when there are 50-100 lines
in a commit. :)

If you are using git, take use of `git add -p` to add specific chunks of code
in git, instead of adding a complete file into git like you're used to, by
doing `git add file1.py`. It's a pretty powerful tool. Spend 15 mins to
understand it and you'll go 'why didn't I know about this till now!'


I am realizing I can actually write 'Git beginner mistakes' as a separate blog
:). But anyway, continuing..

## Spaces after comma and colon

Just make this a habit. It improves your code quality.

Example 1 - bad:

    def create_user(name=name,height=height,weight=weight):
        pass

Example 2 - good:

    def create_user(name=name, height=height, weight=weight):
        pass

Example 3 - bad:

    request_dict = {'name':name,'height':height}

Example 4 - good:

    request_dict = {'name': name, 'height': height}

## Write a lot of comments and docstrings

If something is not abvious from the code, you should write a comment about
it. Also, make a habit of writing docstrings to functions/methods, even if it
is just a single line. Example:

    def create_user(name, height=None, weight=None):
        '''Create a user entry in database. Returns database object created for user.'''
        # Logic to create entry into db and return db object

## Write TODOs literally everywhere

While writing code, you realize it can be improved, but you don't want to
improve it right now as it is not that important thing at this moment. What
should you do? Just create a TODO! I find this a very good compromise for
smaller tasks, instead of creating an issue in an issue/bug tracker. TODOs can
be written for improving on docstrings (`TODO(rushiagr): add more description
here`), a more optimised version of code (e.g. `TODO(rushiagr): can be
optimised by using list comprehensions instead of the 'for' loop`), or a
refactoring (`TODO(rushiagr): create a common method of this thrice-duplicated
code`). The parenteses specify who wrote the TODO, which can be helpful if
somebody wants to fix the todo but doesn't know who to contact for more
information. But you can leave this part out too.

I use TODOs so much that I created a Vim abbreviation for it, so that as soon
as I type `#t` followed by a space, it auto-completes it to `# TODO(rushiagr):
`. If you're using Vim, you can do by adding this line to your `.vimrc`:

    :abbreviate #t # TODO(rushiagr):

That's all folks! :) Do let me know what you feel about this blog by
commenting. Do you know something which you too encountered a lot which is not
present in this article?  Or you disagree with something I wrote?
