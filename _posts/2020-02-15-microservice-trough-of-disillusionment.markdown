---
layout: default
title: "Entering the Microservice Trough of Disillusionment: What Did We Learn?"
categories: microservices
excerpt_separator: <!--more-->
---
It *seems* like we are beginning to enter the "Trough of Disillusionment" in the [hype cycle](https://en.wikipedia.org/wiki/Hype_cycle) for microservices. We're starting to see more and more frustration with the microservices approach and how they have failed to meet our expectations. It doesn't take a lot of Googling to see that. Let's take a step back and see what we can learn from the last handful of years of microservices. Then we can be equipped to make decisions regarding future trends, hopefully with a healthier perspective.

<!--more-->

### Understanding Your Use Cases
Often times we like to cite the benefits of using one tool / technique / architecture, but fail to ground those benefits in real needs of the business. It's like we are in a car dealership in San Diego and hear the pitch from the sales person: "This vehicle is equipped with remote start, so you can heat up your car from the warmth of your home! And it only costs an extra $1000!"

"Wow", you think, "that's amazing! Heating up my car without having to walk outside is so cool! I should get that in case I ever decide to move somewhere cold." However, you fail to ask yourself questions like:
- What's the likelihood of me moving to somewhere cold?
- Even if I do move somewhere cold, will it be before I get another car?
- Can I afford an extra $1000?
- Can I add it later if I do decide I need it?

This is what we (developers) love to do with technology. We see the benefits that some new shiny technology has and we want to start using it. We often neglect to ground those benefits in reality and ask ourselves if those solutions are actually solving problems that we have. Microservices are no different, for a while it was the shiny new thing and everyone wanted in on the action. However, many failed to ground the promises of microservices in the actual problems their business has.

### Sensible Dependencies
You've just finished a long day at work, you come home and just want to kick your feet up and enjoy a frozen pizza. You turn on the oven and throw in the pizza. Then you try to turn on the sink, and the water is not running! After a long investigation, you determine that somehow turning the oven on causes your faucet to break. How confusing, who would make those two things connected?!?

Obviously, this is a bit of a silly example. It doesn't make sense to have the oven connected to your sink. But this is what we do in software *all* the time. We add dependencies where they don't make sense, do the quick fix, and say we'll fix it when we have more time.

In microservices, if a service does not have [independent value](TODO) and is too tightly coupled to others, you can quickly create a situation like the analogy above. Microservices have reminded us the importance of having sensible dependency hierarchies.

### Interface-First Development
>
> Note: In this section, although I use a C# interface as an example, I'm referring to interfaces as the *concept*, i.e. an entry point into some system. It could be a REST API, gRPC interface, language interface, UI screen, etc.

As developers, we sometimes dive straight into implementation without thinking too much about how some component is to be interacted with. In microservices, however, there are too many boundary points to avoid thinking about "user experience" (user being anyone using your code, whether it's via a REST API, language interface, or UI). It's very important to design the interface upfront and get feedback on it before diving into implementation. You might say, "but how can you know what the interface is without diving into the implementation, won't the interface emerge from that?". No! An interface may emerge from it, but not a well-designed one. 

Imagine if a restaraunt approached their menu this way. Chef Bob goes into the kitchen and starts experimenting with new recipes. Once he finds one he likes, he tosses together a menu that makes sense to him based on the recipe he likes. And it reads like this:
```
1 tortilla
1/2 shredded chicken breast
1 cup shredded cheese
1 cup green chili sauce
1 tomato diced
```
In code, one could imagine the interface might look something like this:
```csharp
public interface IMenu
{
    void OrderEnchilada(int tortillas, double chickenBreasts, int cupsOfCheese, int cupsOfGreenChili, int tomatoes);
}
```
When you focus on the implementation first, there is temptation to pass the burden of parameters onto the consumer. In the case of this enchilda, how would one go about ordering this? What happens if I order multiple tortillas but don't double the chicken? Will it mess up the recipe if I remove tomatoes?

Now, let's say I somehow figured out how to navigate this menu, but the enchilada I end up with isn't what I expect. I submit a complaint and the answer I'm given is: "You ordered incorrectly! How silly of you to expect that your enchilada would turn out well if you added 5 tomatoes!". Essentially, I'm told that it was user error. And in some ways, this is true. However, I'm unlikely to go back to this restaurant. 

How might we improve this interface? Well, after taking the customer's input into consideration you discover a couple of things:
- The customers *are* interested in knowing the main ingredients of the enchilada, but not necessarily the brand of tortilla or green chili.
- The customers want the chef to decide the best proportions for the recipe, because they lack adequate knowledge in this area.
- Some customers with dietary restrictions want the flexibility to customize the order.

Armed with this information, a new interface might look something like this:
```csharp
public interface IMenu
{
    /// <summary>
    /// Tortillas stuffed with shredded chicken and smother in green chili and melted cheese.
    /// </summary>
    /// <param name="enchiladaCount">The number of enchiladas to order.</param>
    void OrderEnchiladas(int enchiladaCount);

    /// <summary>
    /// Regular enchiladas made dairy free.
    /// </summary>
    /// <param name="enchiladaCount">The number of enchiladas to order.</param>
    void OrderEnchiladasDairyFree(int enchiladaCount);

    /// <summary>
    /// Regular enchiladas with avocado slices on top.
    /// </summary>
    /// <param name="enchiladaCount">The number of enchiladas to order.</param>
    void OrderEnchiladasWithAvocado(int enchiladaCount);

    /// <summary>
    /// Build your own enchiladas.
    /// </summary>
    /// <param name="enchiladaCount">The number of enchiladas to order.</param>
    /// <param name="options">Options for customizing your enchilada.</param>
    void OrderEnchiladasWithOptions(int enchiladaCount, EnchiladaOptions options);
}
```
We have the default way of ordering enchiladas as-is, a couple of methods for ordering with common modifications, and a method that supports more in-depth customization. Each method is documented to inform the consumer what purpose it serves. Now, we can direct the customer to the appropriate method based on their specific needs.

Microservices create a lot of places that require some form of an interface. Poorly designed interfaces lead to dissatisfied consumers. Internal consumers are unlikely to rely on you in the future, and external consumers are unlikely to continue using your services.

---
Let's apply these lessons to approach technology decisions with clearer vision. We don't have to stay in the Trough of Disullisionment, and if we play our cards right, perhaps we can end up on the Plateau of Productivity sooner rather later.