# Headspring Enumeration #


### Basic Usage ###

```csharp
public class Color : Enumeration<Color, int>
{
    public static readonly Color Red = new Color(1, "Red");
    public static readonly Color Blue = new Color(2, "Blue");
    public static readonly Color Green = new Color(3, "Green");

    private Color(int value, string displayName) : base(value, displayName) { }
}

public void Using_the_color_example()
{
    Color myFavorite = Color.Blue;

    Color leastFavorite = Color.Green;
}

public class Color : Enumeration<Color, string>
{
    public static readonly Color Red = new Color("RED", "Red");
    public static readonly Color Blue = new Color("BLUE", "Blue");
    public static readonly Color Green = new Color("GREEN", "Green");

    private Color(string value, string displayName) : base(value, displayName) { }
}

public void Using_the_color_example()
{
    Color myFavorite = Color.Blue;

    Color leastFavorite = Color.Green;
}
```

### Creating a select list ###

```csharp
public class State : Enumeration<State, int>
{
    public static readonly State Alabama = new State(1, "AL", "Alabama");
    public static readonly State Alaska = new State(2, "AK", "Alaska");
    // .. many more
    public static readonly State Wyoming = new State(3, "WY", "Wyoming");

    private State(int value, string displayName, string description) : base(value, displayName)
    {
        Description = description;
    }

    public string Description { get; private set; }
}

public IEnumerable<SelectListItem> Creating_a_select_list(State selected)
{
    return State.GetAll().Select(
        x => new SelectListItem
        {
            Selected = x == selected,
            Text = x.Description,
            Value = x.Value.ToString()
        });
}
```

### As dispatch table ###

```csharp
public class Calculation : Enumeration<Calculation, int>
{
    public static readonly Calculation Add = new Calculation(1, "Add", (left, right) => left + right);
    public static readonly Calculation Subtract = new Calculation(2, "Subtract", (left, right) => left - right);
    public static readonly Calculation Multiply = new Calculation(3, "Multiply", (left, right) => left * right);

    private Calculation(int value, string displayName, Func<int, int, int> calculation)
        : base(value, displayName)
    {
        Go = calculation;
    }

    public Func<int, int, int> Go { get; private set; }
}

public int Using_the_calculator(Calculation input)
{
    return input.Go(2, 4);
}
```

### Helpful query methods ###

```csharp
public class Role : Enumeration<Role, int>
{
    public static readonly Role System = new Role(999, "System", "System", true);
    public static readonly Role Manager = new Role(1, "Manager", "Michelle", false);
    public static readonly Role Employee = new Role(2, "Employee", "Eric", false);
    public static readonly Role HR = new Role(3, "Human Resources", "Harry", false);

    private Role(int value, string displayName, string personaName, bool testrole)
        : base(value, displayName)
    {
        PersonaName = personaName;
        TestRole = testrole;
    }

    public string PersonaName { get; private set; }
    public bool TestRole { get; private set; }

    public static IEnumerable<Role> GetAllProductionRoles()
    {
        return GetAll().Where(r => !r.TestRole);
    }
}
```
