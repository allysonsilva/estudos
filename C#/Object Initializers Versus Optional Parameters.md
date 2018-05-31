# Object Initializers Versus Optional Parameters

Instead of using object initializers, we could make Bunny’s constructor accept optional parameters:

```csharp
public Bunny(string name, bool likesCarrots = false, bool likesHumans = false)
{
    Name = name;
    LikesCarrots = likesCarrots;
    LikesHumans = likesHumans;
}
```

This would allow us to construct a Bunny as follows:

```csharp
Bunny b1 = new Bunny(name: "Bo", likesCarrots: true);
```

An advantage of this approach is that we could make Bunny’s fields (or *properties*, as we’ll explain shortly) read-only if we choose. Making fields or properties read-only is good practice when there’s no valid reason for them to change throughout the life of the object.

The disadvantage in this approach is that each optional parameter value is baked into the calling site. In other words, C# translates our constructor call into this:

```csharp
Bunny b1 = new Bunny("Bo", true, false);
```

This can be problematic if we instantiate the Bunny class from another assembly, and later modify Bunny by adding another optional parameter—such as likesCats. Unless the referencing assembly is also recompiled, it will continue to call the (now nonexistent) constructor with three parameters and fail at runtime. (A subtler problem is that if we changed the value of one of the optional parameters, callers in other assemblies would continue to use the old optional value until they were recompiled.)

Hence, you should exercise caution with optional parameters in public functions if you want to offer binary compatibility between assembly versions.
