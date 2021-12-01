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

