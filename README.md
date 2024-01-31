# cse335
import SwiftUI

var age: Int?
let earthGravity = 9.81

func rfmquestion(rfm: Double) -> String{
    switch rfm {
    case 0...20:
        return "You are Underfat"
    case 20.01...35:
        return "You are Healthy"
    case 35.01...42:
        return "You are Overfat"
    default:
        return "You are Obese"
    }
}

func reverse(str: String) -> String {
    var reversed = ""
    for character in str {
        reversed = "\(character)\(reversed)"
    }
    return reversed
}

func reverseword(in sentence: String) -> String {
    let words = sentence.split(separator: " ")
    let reversedW = words.map { word -> String in
        var reversedW = ""
        for character in word {
            reversedW = "\(character)\(reversedW)"
        }
        return reversedW
    }
    return reversedW.joined(separator: " ")
}

func sumquestion(numbers: [Int]) -> (oddSum: Int, evenSum: Int){
    var oddSum = 0
    var evensum = 0
    for number in numbers where number > 0 {
        if number % 2 == 0 {
            evensum += number
        } else{
            oddSum += number
        }
    
    }
    return (oddSum, evensum)
}

func ShortestAndLongestquestion(strings: [String]) -> (shortest: String?, longest: String?) {
    guard !strings.isEmpty else { return (nil, nil) }
    var shortest = strings.first!
    var longest = strings.first!
    
    for string in strings {
        if string.count < shortest.count { shortest = string }
        if string.count > longest.count { longest = string }
    }
    return (shortest, longest)
}

func sequentialSearch(in array: [Int], for value: Int) -> Bool {
    for element in array where element == value {
        return true
    }
    return false
}

class CityStatistics {
    var name: String
    var population: Int
    var longitude: Double
    var latitude: Double

    init(name: String, population: Int, longitude: Double, latitude: Double) {
        self.name = name
        self.population = population
        self.longitude = longitude
        self.latitude = latitude
    }

    func getPopulation() -> Int {
        return self.population
    }

    func getLatitude() -> Double {
        return self.latitude
    }
}

// Declaring the dictionary
var cityStatsDictionary = [String: CityStatistics]()
// Function to populate dictionary
func populateCityStatsDictionary() {
    cityStatsDictionary["Paris"] = CityStatistics(name: "Paris", population: 2140526, longitude: 2.3522, latitude: 48.8566)
    cityStatsDictionary["Tokyo"] = CityStatistics(name: "Tokyo", population: 13929286, longitude: 139.6917, latitude: 35.6895)
    cityStatsDictionary["Berlin"] = CityStatistics(name: "Berlin", population: 3645000, longitude: 13.4050, latitude: 52.5200)
    cityStatsDictionary["Sydney"] = CityStatistics(name: "Sydney", population: 5312163, longitude: 151.2093, latitude: -33.8688)
    cityStatsDictionary["Cape Town"] = CityStatistics(name: "Cape Town", population: 433688, longitude: 18.4241, latitude: -33.9249)
}

func findCityWithLargestPopulation() -> String {
    var largestPopulation = 0
    var cityWithLargestPopulation = ""

    for (cityName, statistics) in cityStatsDictionary {
        if statistics.getPopulation() > largestPopulation {
            largestPopulation = statistics.getPopulation()
            cityWithLargestPopulation = cityName
        }
    }

    return cityWithLargestPopulation.isEmpty ? "Not available" : "\(cityWithLargestPopulation) with a population of \(largestPopulation)"
}

func findNorthernmostCity() -> String {
    var highestLatitude = -Double.infinity
    var northernmostCity = ""

    for (cityName, statistics) in cityStatsDictionary {
        if statistics.getLatitude() > highestLatitude {
            highestLatitude = statistics.getLatitude()
            northernmostCity = cityName
        }
    }

    return northernmostCity.isEmpty ? "Not available" : "\(northernmostCity) at latitude \(highestLatitude)"
}


func findTopStudent(students: [[String: Any]]) -> String {
    guard !students.isEmpty else { return "No students available" }
    var topStudent: (firstName: String, lastName: String, gpa: Double) = ("", "", 0.0)

    for student in students {
        if let gpa = student["gpa"] as? Double, gpa > topStudent.gpa {
            topStudent.gpa = gpa
            topStudent.firstName = student["firstName"] as? String ?? ""
            topStudent.lastName = student["lastName"] as? String ?? ""
        }
    }

    return topStudent.gpa > 0 ? "\(topStudent.firstName) \(topStudent.lastName)" : "No valid GPA records found"
}

struct ContentView: View {
    @State private var reversedString = ""
    @State private var reversedWordString = ""
    @State private var oddEvenSums: (oddSum: Int, evenSum: Int) = (0, 0)
    @State private var shortestLongestWords: (shortest: String?, longest: String?) = (nil, nil)
    @State private var searchResult = false
    @State private var largestPopulationCity = ""
    @State private var northernmostCity = ""
    @State private var topStudentName = ""

    var body: some View {
        VStack {
            Text(rfmquestion(rfm: 22))
            Text("Reversed String: \(reversedString)")
            Text("Reversed Each Word: \(reversedWordString)")
            Text("Sum Odd Even: Odd - \(oddEvenSums.oddSum), Even - \(oddEvenSums.evenSum)")
            Text("Shortest and Longest Words: Shortest - \(shortestLongestWords.shortest ?? ""), Longest - \(shortestLongestWords.longest ?? "")")
            Text("Sequential Search Result: \(searchResult ? "Found" : "Not Found")")
            Text("Largest Population: \(largestPopulationCity)")
            Text("Northernmost City: \(northernmostCity)")
            Text("Top Student: \(topStudentName)")
        }
        .onAppear {
            reversedString = reverse(str: "Hello, World!")
            reversedWordString = reverseword(in: "Hello World")
            oddEvenSums = sumquestion(numbers: [1, 2, 3, 4, 5, 6])
            shortestLongestWords = ShortestAndLongestquestion(strings: ["Swift", "Programming", "Language"])
            searchResult = sequentialSearch(in: [1, 2, 3, 4, 5], for: 4)
            populateCityStatsDictionary()
            largestPopulationCity = findCityWithLargestPopulation()
            northernmostCity = findNorthernmostCity()
            
            let students: [[String: Any]] = [
                ["firstName": "John", "lastName": "Wilson", "gpa": 2.4],
                ["firstName": "Nancy", "lastName": "Smith", "gpa": 3.5],
                ["firstName": "Michael", "lastName": "Liu", "gpa": 3.1]
            ]
            topStudentName = findTopStudent(students: students)
        }
    }
}
