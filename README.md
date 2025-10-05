# Mission Control Simulation API

The **Mission Control Simulation API** is a fun practice API designed for students and developers learning how to work with APIs. It provides a set of endpoints for simulation of exploring space missions, agencies, spacecraft, and crews.  

Instead of using the same old boring APIs, this project gives you something cool and space-themed to experiment with while learning concepts like HTTP requests, JSON parsing, and data modeling.

---

## Base URLs

### v2 — Simplified model with direct data  
[`https://missioncontrol-e832.onrender.com/api/v2/Missions`](https://missioncontrol-e832.onrender.com/api/v2/Missions)

### v1 — Includes relationships and reference tables  
[`https://missioncontrol-e832.onrender.com/api/v1/Missions`](https://missioncontrol-e832.onrender.com/api/v1/Missions)





---

## Example C# Usage

Here’s a simple console app that calls the API and displays mission data:

```csharp
using System;
using System.Collections;
using System.Text.Json;

namespace API
{
    class Program
    {
        public static async Task Main(string[] args)
        {
            Console.WriteLine(DateTime.Today.ToString("D"));

            Console.WriteLine($"\nMission Simulation Control API\n");

            // Base URL for the mission API endpoint
            string url = "https://missioncontrol-e832.onrender.com/api/v1/Missions";

            // Init HTTP client to send and receive web requests
            HttpClient client = new HttpClient();


            // Set JSON options to ignore property name casing when deserializing
            var options = new JsonSerializerOptions()
            {
                PropertyNameCaseInsensitive = true
            };

            try
            {
                // Send GET request to the API
                var response = await client.GetAsync(url);


                // Check if the response was successful: status code 200
                if (response.IsSuccessStatusCode)
                {
                    // Grabs the raw JSON from the response body
                    var json = await response.Content.ReadAsStringAsync();

                    // Deserialize JSON into a list of MissionMain objects
                    var missionJson = JsonSerializer.Deserialize<List<MissionMain>>(json);

                    Console.WriteLine($"{"ID(GUID)",-42} {"MissionName",-42} {"Agency",-25} ");
                    Console.WriteLine(new string('*', 100));


                    // Loop through each mission and print its details in formatted columns
                    foreach (var item in missionJson)
                    {
                        Console.WriteLine($"{item.missionId,-42} {item.name,-42} {item.agency,-25}");

                    }
                }
            }
            catch (Exception ex)
            {

                Console.WriteLine($"Error message: {ex.Message}");
                throw;
            }
        }
        // Classes below represent the structure of the JSON returned by the API
        public class Crew
        {
            public string crewId { get; set; }
            public string astronautName { get; set; }
            public string nationality { get; set; }
        }

        public class MissionType
        {
            public string missionTypeId { get; set; }
            public string name { get; set; }
        }
        public class MissionMain
        {
            public string missionId { get; set; }
            public string name { get; set; }
            public string agency { get; set; }
            public DateTime launchDate { get; set; }
            public Status status { get; set; }
            public MissionType missionType { get; set; }
            public Spacecraft spacecraft { get; set; }
            public Crew crew { get; set; }
        }

        public class Spacecraft
        {
            public string spacecraftId { get; set; }
            public string name { get; set; }
            public string type { get; set; }
            public string manufacturer { get; set; }
        }

        public class Status
        {
            public string statusId { get; set; }
            public string name { get; set; }
        }
    }
}
