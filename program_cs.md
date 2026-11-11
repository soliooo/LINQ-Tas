using System;
using System.Collections.Generic;
using System.Linq;

class Program
{
    static void Main(string[] args)
    {
        List<Book> BookList = new List<Book>
        {
            new Book { BookId = 1, Title = "C# Programming", Genre = "Programming", Price = 45.0m, CopiesInStock = 5 },
            new Book { BookId = 2, Title = "Advanced LINQ", Genre = "Programming", Price = 55.0m, CopiesInStock = 0 },
            new Book { BookId = 3, Title = "Clean Code", Genre = "Software Engineering", Price = 60.0m, CopiesInStock = 10 },
            new Book { BookId = 4, Title = "Design Patterns", Genre = "Software Engineering", Price = 75.0m, CopiesInStock = 2 },
            new Book { BookId = 5, Title = "Database Systems", Genre = "Database", Price = 40.0m, CopiesInStock = 0 },
            new Book { BookId = 6, Title = "SQL Masterclass", Genre = "Database", Price = 50.0m, CopiesInStock = 8 },
            new Book { BookId = 7, Title = "AI Foundations", Genre = "Artificial Intelligence", Price = 90.0m, CopiesInStock = 4 },
            new Book { BookId = 8, Title = "Machine Learning", Genre = "Artificial Intelligence", Price = 95.0m, CopiesInStock = 0 },
            new Book { BookId = 9, Title = "Deep Learning", Genre = "Artificial Intelligence", Price = 110.0m, CopiesInStock = 3 },
            new Book { BookId = 10, Title = "HTML & CSS", Genre = "Web Development", Price = 25.0m, CopiesInStock = 15 },
            new Book { BookId = 11, Title = "JavaScript Guide", Genre = "Web Development", Price = 35.0m, CopiesInStock = 12 },
            new Book { BookId = 12, Title = "React Native", Genre = "Web Development", Price = 50.0m, CopiesInStock = 6 },
            new Book { BookId = 13, Title = "Node.js Essentials", Genre = "Web Development", Price = 45.0m, CopiesInStock = 0 },
            new Book { BookId = 14, Title = "Docker & Kubernetes", Genre = "DevOps", Price = 65.0m, CopiesInStock = 7 },
            new Book { BookId = 15, Title = "CI/CD Pipelines", Genre = "DevOps", Price = 70.0m, CopiesInStock = 1 }
        };

        #region Part 1: Ordering
        var q1_fluent = BookList.OrderBy(b => b.Price);
        var q1_query = from b in BookList orderby b.Price select b;

        var q2_fluent = BookList.OrderByDescending(b => b.CopiesInStock).ThenBy(b => b.Price);
        var q2_query = from b in BookList orderby b.CopiesInStock descending, b.Price ascending select b;

        var q3_fluent = BookList.OrderBy(b => b.Genre).ThenBy(b => b.Title);
        var q3_query = from b in BookList orderby b.Genre, b.Title select b;
        #endregion

        #region Part 2: Element Operators
        var q4_fluent = BookList.First(b => b.Price > 50);
        var q4_query = (from b in BookList where b.Price > 50 select b).First();

        var q5_fluent = BookList.LastOrDefault(b => b.CopiesInStock == 0);
        var q5_query = (from b in BookList where b.CopiesInStock == 0 select b).LastOrDefault();

        var q6_fixed = BookList.SingleOrDefault(b => b.BookId == 1);

        List<Book> emptyList = new List<Book>();
        var q7_fixed = emptyList.FirstOrDefault();
        #endregion

        #region Part 3: Casting
        var q8_fluent = BookList.Where(b => b.CopiesInStock == 0).ToDictionary(b => b.BookId, b => b.Title);
        var q8_query = (from b in BookList where b.CopiesInStock == 0 select b).ToDictionary(b => b.BookId, b => b.Title);

        var q9_fluent = BookList.Select(b => b.Genre).ToHashSet();
        var q9_query = (from b in BookList select b.Genre).ToHashSet();
        #endregion

        #region Part 4: Aggregate
        var q10_fluent = BookList.Count(b => b.CopiesInStock < 5);
        var q10_query = (from b in BookList where b.CopiesInStock < 5 select b).Count();

        var mostExpensive = BookList.MaxBy(b => b.Price);
        var leastExpensive = BookList.MinBy(b => b.Price);

        var q12_fluent = BookList.GroupBy(b => b.Genre).Select(g => new { Genre = g.Key, AvgPrice = g.Average(b => b.Price) });
        var q12_query = from b in BookList group b by b.Genre into g select new { Genre = g.Key, AvgPrice = g.Average(b => b.Price) };

        var q13_fluent = BookList.Sum(b => b.Price * b.CopiesInStock);
        var q13_query = (from b in BookList select b.Price * b.CopiesInStock).Sum();

        var q14 = BookList.Take(3).Select(b => b.Title).Aggregate((acc, next) => $"{acc} {next}");
        #endregion

        #region Part 5: Generation
        var q15 = Enumerable.Range(1, 100).Where(n => n % 2 == 0 && n % 5 == 0);
        var q16 = Enumerable.Repeat("LINQ", 5).ToArray();
        #endregion

        #region Part 6: Set Operators
        var seq1 = Enumerable.Range(0, 20);
        var seq2 = Enumerable.Range(10, 20);

        var unionRes = seq1.Union(seq2);
        var intersectRes = seq1.Intersect(seq2);
        var exceptRes = seq1.Except(seq2);
        var concatRes = seq1.Concat(seq2);

        List<Book> listA = new List<Book> { new Book { BookId = 1, Title = "C#", Genre = "Prog", Price = 10, CopiesInStock = 5 } };
        List<Book> listB = new List<Book> { new Book { BookId = 1, Title = "C#", Genre = "Prog", Price = 10, CopiesInStock = 5 } };
        var fixedIntersect = listA.Intersect(listB, new BookComparer());

        var intersectByRes = listA.IntersectBy(listB.Select(b => b.BookId), b => b.BookId);
        #endregion

        #region Part 7: Quantifiers & Zip
        bool anyPriceZero = BookList.Any(b => b.Price == 0);
        bool allPriceAboveZero = BookList.All(b => b.Price > 0);

        bool anyOutOfStock = BookList.Any(b => b.CopiesInStock == 0);
        bool allProgInStock = BookList.Where(b => b.Genre == "Programming").All(b => b.CopiesInStock > 0);

        string[] names = { "Omar", "Salma", "Youssef", "Mona" };
        int[] grades = { 90, 85, 70 };
        string[] tracks = { ".NET", "Web", "AI" };

        var zip1 = names.Zip(grades);
        var zip2 = names.Zip(grades, tracks);
        var zip3 = names.Zip(grades, (n, g) => $"{n}: {g}");
        #endregion

        #region Part 8: Grouping
        var q23_fluent = BookList.GroupBy(b => b.Genre);
        var q23_query = from b in BookList group b by b.Genre;

        var q24 = from b in BookList 
                  where b.CopiesInStock > 0 
                  group b by b.Genre into g 
                  where g.Count() > 3 
                  select g;

        var q25_fluent = BookList.GroupBy(b => b.Genre)
                                 .Select(g => new { Genre = g.Key, Count = g.Count(), MaxPrice = g.Max(b => b.Price) });
        var q25_query = from b in BookList 
                        group b by b.Genre into g 
                        select new { Genre = g.Key, Count = g.Count(), MaxPrice = g.Max(b => b.Price) };
        #endregion

        #region Part 9: Partitioning
        var q26 = BookList.Where(b => b.CopiesInStock == 0).Take(3);

        var page1 = GetPage(BookList, 1, 5);
        var page2 = GetPage(BookList, 2, 5);
        var page3 = GetPage(BookList, 3, 5);

        int[] Numbers = { 6, 5, 2, 4, 9, 8, 7, 3, 1, 0 };
        var takeWhileRes = Numbers.TakeWhile((n, index) => n >= index);
        var skipWhileRes = Numbers.SkipWhile((n, index) => n >= index);
        #endregion

        #region Bonus
        var bonusQuery = BookList.Where(b => b.CopiesInStock > 0)
                                 .GroupBy(b => b.Genre)
                                 .SelectMany(g => g.OrderByDescending(b => b.Price).Take(3));
        #endregion
    }

    static IEnumerable<Book> GetPage(IEnumerable<Book> books, int pageNumber, int pageSize)
    {
        return books.Skip((pageNumber - 1) * pageSize).Take(pageSize);
    }
}

public class Book
{
    public long BookId { get; set; }
    public string Title { get; set; }
    public string Genre { get; set; }
    public decimal Price { get; set; }
    public int CopiesInStock { get; set; }
}

public class BookComparer : IEqualityComparer<Book>
{
    public bool Equals(Book x, Book y)
    {
        if (ReferenceEquals(x, y)) return true;
        if (x is null || y is null) return false;
        return x.BookId == y.BookId && x.Title == y.Title && x.Genre == y.Genre && x.Price == y.Price && x.CopiesInStock == y.CopiesInStock;
    }

    public int GetHashCode(Book obj)
    {
        return HashCode.Combine(obj.BookId, obj.Title, obj.Genre, obj.Price, obj.CopiesInStock);
    }
}