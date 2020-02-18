---
layout: default
title: Exceptions vs Results in C#
categories: csharp
tags: exceptions csharp c#
excerpt_separator: <!--more-->
---
A common topic of discussion at the beginning of projects is error handling, and how to best communicate those errors to end-users. Creating custom exceptions is often the choice, but let's consider some alternatives. 

<!--more-->

## The Problem With Exceptions
Let's say I am on Team B and we rely on some code written by Team A. It contains a utility for posting to various social media sites. In it, there is an interface that looks like this:
```csharp
public interface ISocialMediaPoster
{
    void Post(string content);
}
```
At first, it looks simple and easy to use. However, what happens if it failed to post to the social media site? Based on this interface, it's unclear. Perhaps Team A is very thorough, and they add good xml-doc comments:
```csharp
public interface ISocialMediaPoster
{
    /// ...
    /// <exception cref="SocialMedia.FailedToPostException">Thrown when social media site rejects the post.</exception>
    IPost Post(string content);
}
```
That's better, now we know that it will throw an exception if it fails to post. However, now we are relying on a manual process. We are *hoping* that Team A has listed all possible exceptions, and that they will continue to update it.

For now, let's assume that Team A is meticulous about keeping their documentation up-to-date. I (the consumer) have a few options to make sure I'm not surprised:
1) Anytime I upgrade the package, check the documentation and make sure no new exceptions were added.
2) Look at Team A's code (hopefully you have read access at least) and make sure nothing surprising is added.
3) Hope Team A doesn't add any new exceptions.
4) Catch all exceptions:
```csharp
ISocialMediaPoster socialMediaPoster = // ...
try
{
    socialMediaPoster.Post("My dog ate my homework");
}
catch (SocialMedia.FailedToPostException e)
{
    // ...
}
catch (Exception e)
{
    // ...
}
```

None of these options are sounding too appealing. Option #1 is not only tedious, but also not foolproof. Option #2 is also tedious and annoying. Option #3 might be ok if you have good automated testing in place. Option #4 is probably the best choice, but still unsatisfying. There's still the possibility that we are catching an exception that wasn't intended to be caught.

The root of the problem here is that **the exceptions are not a part of the contract, they are implementation details**. Team B is not solely dependent on the abstraction, we're also dependent on the implementation.

## Alternatives
How can we improve the interface so the consumer is not dependent on implementation details? Here are some alternatives.

### Returning a Tuple
Using a tuple, let's see how we can refactor the interface:
```csharp
public enum ErrorType
{
    Unknown,
    SocialMediaSiteUnavailable,
    InvalidContent
}

public interface ISocialMediaPoster
{
    (IPost, ErrorType?) Post(string content);
}
```
And using it looks like this:
```csharp
ISocialMediaPoster socialMediaPoster = // ...
(IPost post, ErrorType? error) = socialMediaPoster.Post("My dog ate my homework");

if (error != null)
{
    // handle error
}

// happy path
```
Or instead of an `enum`, you could do something like this:
```csharp
public class SocialMediaError
{
    public string SiteType {get; set;}

    public List<string> Errors {get; set;}

    public string UserId {get; set;}

    // etc.
}

public interface ISocialMediaPoster
{
    (IPost, SocialMediaError) Post(string content);
}
```

### Returning a Result
Another option is to return a single object that contains both the successful response and errors.
```csharp
public class SocialMediaResult
{
    bool Success => Errors.Count == 0;

    public string SiteType {get; set;}

    public List<string> Errors {get; set;}

    public string UserId {get; set;}

    public IPost NewPost {get; set;}
}

public interface ISocialMediaPoster
{
    SocialMediaResult Post(string content);
}

// ...
SocialMediaResult result = socialMediaPoster.Post("My dog ate my homework");

if (!result.Success)
{
    // handle error
}

// happy path
```

### Try-Parse Pattern
Sometimes you want to give the caller a choice in whether to have a method throw an exception or not. For example this throws an exception:
```csharp
int parsedInt = int.Parse("$%@");
```
This does not:
```csharp
bool success = int.TryParse("$%@", out int parsedInt);
```
This pattern is especially useful in frameworks, when you don't want to make a decision for the consumer if something is exceptional or not.

---

What do all of these techniques have in common? They all make errors part of the contract (i.e. interface). It's clear to the consumer all the possible errors that can occur, *without having any knowledge of the implementation details*. Here are some other advantages:
- Error handling becomes a 1st-class citizen. The caller of the method is forced to handle the error. With an exception, it's easy to skip.
- Errors are part of the contract. Now we know that any exception that occurs is truly exceptional.
- Consumers can depend solely on the abstraction rather than the custom exceptions. 

## Are Exceptions Always Bad?
No! Exceptions certainly have their place. For example, throwing `ArgumentException` or `ArgumentNullException` is useful when an argument passed in makes it impossible for the method to complete the action it promises to. Or, perhaps something unrecoverable occurred, such as the social media site being down permanently. The rule of thumb here is that *exceptions should be exceptional, don't use them if the error can be expected as part of the flow of the program*. If something exceptional occurred, then it is appropriate to use.

---

Consider using some of the techniques above in your project to help improve the clarity and quality of your code.
