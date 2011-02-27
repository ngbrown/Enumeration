# Headspring Enumeration #


### Basic Usage ###

    public class Color : Enumeration
    {
        public static Color Red = new Color(1, "Red");
        public static Color Blue = new Color(2, "Blue");
        public static Color Green = new Color(3, "Green");
        public Color() {}
        public Color(int value, string displayName) : base(value, displayName) {}
    }

    public void Using_the_color_example()
    {
        Color myFavorite = Color.Blue;

        Color leastFavorite = Color.Green;
    }

### Creating a select list ###

    public class State : Enumeration
    {
        public static State Alabama = new State(1, "AL", "Alabama");
        public static State Alaska = new State(2, "AK", "Alaska");
        // .. many more
        public static State Wyoming = new State(3, "WY", "Wyoming");

        public State() {}

        public State(int value, string displayName, string description) : base(value, displayName)
        {
            Description = description;
        }

        public string Description { get; private set; }
    }

    public IEnumerable<SelectListItem> Creating_a_select_list(State selected)
    {
        return Enumeration.GetAll<State>().Select(
            x => new SelectListItem
            {
                Selected = x == selected,
                Text = x.Description,
                Value = x.Value.ToString()
            });
    }

### As dispatch table ###
	
    public class Calculation : Enumeration
    {
        public static Calculation Add = new Calculation(1, "Add", (right, left) => right + left);
        public static Calculation Subtract = new Calculation(2, "Subtract", (right, left) => right - left);
        public static Calculation Multiply = new Calculation(3, "Multiply", (right, left) => right * left);
        public Calculation() { }

        public Calculation(int value, string displayName, Func<int, int, int> calculation)
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

### Helpful query methods ###

    public class Role : Enumeration
    {
        public static readonly Role System = new Role(999, "System", "System", true);
        public static readonly Role Manager = new Role(1, "Manager", "Michelle", false);
        public static readonly Role Employee = new Role(2, "Employee", "Eric", false);
        public static readonly Role HR = new Role(3, "Human Resources", "Harry", false);

        public Role(int value, string displayName, string personaName, bool testrole)
            : base(value, displayName)
        {
            PersonaName = personaName;
            TestRole = testrole;
        }

        public Role() { }
        public string PersonaName { get; private set; }
        public bool TestRole { get; private set; }

        public static IEnumerable<Role> GetAllProductionRoles()
        {
            return GetAll<Role>().Where(r => !r.TestRole);
        }
    }