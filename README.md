# EntityFrameworkAdoNet
intro to EF and AdoNet

///

Entity Framework is an open-source ORM framework for .NET applications supported by Microsoft. It enables developers to work with data using objects of domain specific classes without focusing on the underlying database tables and columns where this data is stored. With the Entity Framework, developers can work at a higher level of abstraction when they deal with data, and can create and maintain data-oriented applications with less code compared with traditional applications.

Official Definition: “Entity Framework is an object-relational mapper (O/RM) that enables .NET developers to work with a database using .NET objects. It eliminates the need for most of the data-access code that developers usually need to write.”

using Microsoft.Data.SqlClient;
using System;

namespace CSharpDbDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            string connectionString = @"Server=.\SQLEXPRESS;Integrated Security=true;Database=SoftUni";

            using (var connection = new SqlConnection(connectionString))
            {
                connection.Open();
                //string query = "SELECT COUNT(*) FROM Employees";

                SqlCommand sqlCommand = new SqlCommand("UPDATE Employees SET Salary = Salary + 0.12", connection);
                var rowsAffected = sqlCommand.ExecuteReader();
                Console.WriteLine(rowsAffected);

            } 
        }
    }
}




///////////////


using System;
using System.Collections.Generic;
using Microsoft.Data.SqlClient;

namespace AdoNet
{
    public class Program
    {
        const string SqlConnectionString =
                              @"Server=BLA-PC\SQLEXPRESS;Database=MinionsDB;Integrated Security=true";

        public static void Main(string[] args)
        {
            using (var connection = new SqlConnection(SqlConnectionString)) 
            {
                connection.Open();

                string countryName = Console.ReadLine();

                string updateTownNamesQuery = @"UPDATE Towns 
                                                   SET Name = UPPER(Name) 
                                                 WHERE Cuntrycode = 
                                               (SELECT c.Id FROM Countries AS c WHERE c.Name = @countryName)";

                string selectTownNamesQuery = @"SELECT t.Name FROM Towns AS t 
                                                  JOIN  Countries AS c ON c.Id = t.CountryCode
                                                 WHERE  c.Name = @countryName";


                using var updateCommand = new SqlCommand(updateTownNamesQuery, connection);
                updateCommand.Parameters.AddWithValue("@countryName", countryName);
                var affectedRows = updateCommand.ExecuteNonQuery();

                if (affectedRows == 0)
                {
                    Console.WriteLine("No town names were affected.");
                }
                else
                {
                    Console.WriteLine($"{affectedRows} town names were affected.");
                    using var selectCommand = new SqlCommand(selectTownNamesQuery, connection);
                    selectCommand.Parameters.AddWithValue("@countryName", countryName);

                    using (var reader = selectCommand.ExecuteReader()) 
                    {

                        var towns = new List<string>();
                        while (reader.Read())
                        {
                            towns.Add((string)reader[0]);
                        }

                        Console.WriteLine($"{string.Join(", ", towns)}");
                       
                    }
                }
            }                
        }
    }
}
