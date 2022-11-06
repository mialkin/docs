# Bogus

[↑ Bogus](https://github.com/bchavez/Bogus) is a simple fake data generator. Bogus is fundamentally a C# port of [↑ faker.js](https://github.com/faker-js/faker).

## Installation

```bash
dotnet add package Bogus
```

## Usage

```csharp
using Bogus;

var players = new List<Employee>();

// Randomizer.Seed = new Random(763512); # Generates deterministic random values

var faker = new Faker<Employee>("ru")
    .RuleFor(x => x.Id, x => x.Random.Guid())
    .RuleFor(x => x.FirstName, x => x.Person.FirstName)
    .RuleFor(x => x.LastName, x => x.Person.LastName)
    .RuleFor(x => x.Age, x => x.Random.Number(1, 99))
    .RuleFor(x => x.Salary, x => x.Finance.Amount(10000, 100000))
    .RuleFor(x => x.BirthDate, x => x.Person.DateOfBirth.Date)
    .RuleFor(x => x.Email, x => x.Person.Email);

for (var i = 0; i < 10; i++)
    players.Add(faker.Generate());

Console.WriteLine(players.Count);

class Employee
{
    public Guid Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public int Age { get; set; }
    public string Email { get; set; }
    public DateTime BirthDate { get; set; }
    public decimal Salary { get; set; }
}
```
