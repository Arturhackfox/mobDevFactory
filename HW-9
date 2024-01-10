import UIKit

protocol Dealership {
    var name: String { get }
    var showroomCapacity: Int { get }
    var stockCars: [Car] { get set }
    var showroomCars: [Car] { get set }
    var cars: [Car] { get set }
    
    func offerAccessories(from array: [Accessorie])
    func presaleService(for car: Car)
    func addToShowroom(car: Car) throws
    func sellCar(_ car: Car)
    func orderCar()
}

protocol Car {
    var serialNum: UUID { get }
    var model: String { get }
    var price: Int { get set }
    var isPresaleServiceDone: Bool { get set }
    var hasAdditionalGear: Bool { get set }
    var releaseYear: Int { get }
    var hasDiscount: Bool { get set }
    
    func changePresaleStatus(to: Bool)
    func makeDiscount(percentage: Double)
}

protocol SpecialOffer {
    func addEmergencyPack()
    func specialOffer(for car: Car) throws
}

extension SpecialOffer {
    func addEmergencyPack() {
        print("Added first aid-kit and fire extinguisher")
    }
    func specialOffer(for car: Car) throws {
        if car.releaseYear < 2024 {
            print("Your car price is: \(car.price)$")
            car.makeDiscount(percentage: 15)
            self.addEmergencyPack()
            print("We made discount for your car \(car.model) it's price now - \(car.price)$")
        } else {
            throw SpecialOfferErrors.latestYear
        }
    }
}

enum SpecialOfferErrors: Error {
    case latestYear
}

enum ShowRoomErrors: Error {
    case alreadyInShowroom
}

// Car Model
class CarModel: Car {
    let serialNum = UUID()
    let model: String
    var price: Int
    var isPresaleServiceDone: Bool
    var hasAdditionalGear: Bool
    var releaseYear: Int
    var hasDiscount: Bool
    
    init(model: String, price: Int, isPresaleServiceDone: Bool, hasAdditionalGear: Bool, year: Int, hasDiscount: Bool) {
        self.model = model
        self.price = price
        self.isPresaleServiceDone = isPresaleServiceDone
        self.hasAdditionalGear = hasAdditionalGear
        self.releaseYear = year
        self.hasDiscount = hasDiscount
    }
    
    func changePresaleStatus(to newStatus: Bool) {
        isPresaleServiceDone = newStatus
        print("presale status changed to - \(newStatus)")
    }
    func makeDiscount(percentage: Double) {
        let originalPrice = price
        let decimalDiscount = percentage / 100
        print(decimalDiscount)
        let discountInUsd = Double(price) * decimalDiscount
        print(discountInUsd)
        self.price -= Int(discountInUsd)
        self.hasDiscount = true
        print("Original price - \(originalPrice)$ changed to discounted - \(price)$")
    }
}

// Dealer - 1
class MercedesDealership: Dealership, SpecialOffer {
    let name: String
    let showroomCapacity: Int
    var stockCars = [Car]()
    var showroomCars = [Car]()
    var cars = [Car]()
    
    init(name: String, showroomCapacity: Int, stockCars: [Car], showroomCars: [Car], cars: [Car]) {
        self.name = name
        self.showroomCapacity = showroomCapacity
        self.stockCars = stockCars
        self.showroomCars = showroomCars
        self.cars = cars
    }
    
    func offerAccessories(from array: [Accessorie]) {
        // принимает массив акксесуаров в качестве параметра.
        // Метод предлагает клиенту купить доп. оборудование.
        let allAccessorieNames = array.map { $0.name }.joined(separator: ", ")
        print("We offer you some usefull accessories: \(allAccessorieNames)")
    }
    func presaleService(for car: Car) {
        // if car hasn't done presale services -> send it to cars array.
        if !car.isPresaleServiceDone {
            car.changePresaleStatus(to: true)
            cars.append(car)
            print("Car: \(car.model) has been added to all cars available list.")
        }
    }
    func addToShowroom(car: Car) throws {
            for (index, currentCar) in stockCars.enumerated() {
                // переганяем машину в автосалон на ней нет скидки
                if currentCar.serialNum == car.serialNum && showroomCars.count < showroomCapacity && !car.hasDiscount {
                    stockCars.remove(at: index)
                    self.presaleService(for: car)
                    showroomCars.append(car)
                    print("New car: \(car.model) has been added to showroom.")
                } else if showroomCars.count < showroomCapacity {
                    // Такой машины ещё нету, добавляем в гараж
                    self.presaleService(for: car)
                    showroomCars.append(car)
                    print("New car: \(car.model) has been added to showroom.")
                } else if car.hasDiscount && currentCar.serialNum == car.serialNum {
                    // В салоне уже есть такая машна, ещё одна не нужна там.
                    throw ShowRoomErrors.alreadyInShowroom
                }
            }
        }
    func sellCar(_ car: Car) {
        for (index, currentCar) in showroomCars.enumerated() {
            if currentCar.serialNum == car.serialNum {
                if currentCar.isPresaleServiceDone {
                    showroomCars.remove(at: index)
                    print("Car \(car.model) sold.")
                    if !currentCar.hasAdditionalGear {
                        print("I can offer you additional gear for your car, do you want to add it to the cart ?")
                    }
                } else if !car.isPresaleServiceDone {
                    self.presaleService(for: car)
                    showroomCars.remove(at: index)
                    print("Car \(car.model) sold.")
                }
            }
        }
    }
    func orderCar() {
        let models = ["s63", "s63Amg", "G-Wagon", "e220d", "e220", "s223"].randomElement() ?? "gazel"
        let randCar = CarModel(model: models, price: Int.random(in: 60_000...350_000), isPresaleServiceDone: false, hasAdditionalGear: false, year: Int.random(in: 2000...2024), hasDiscount: false)
        print("Ordered new car. Model: \(randCar.model), it's price \(randCar.price)$")
    }
}

class FordDealership: Dealership, SpecialOffer {
    let name: String
    let showroomCapacity: Int
    var stockCars = [Car]()
    var showroomCars = [Car]()
    var cars = [Car]()
    
    init(name: String, showroomCapacity: Int, stockCars: [Car], showroomCars: [Car], cars: [Car]) {
        self.name = name
        self.showroomCapacity = showroomCapacity
        self.stockCars = stockCars
        self.showroomCars = showroomCars
        self.cars = cars
    }
    
    func offerAccessories(from array: [Accessorie]) {
        // принимает массив акксесуаров в качестве параметра.
        // Метод предлагает клиенту купить доп. оборудование.
        let allAccessorieNames = array.map { $0.name }.joined(separator: ", ")
        print("We offer you some usefull accessories: \(allAccessorieNames)")
    }
    func presaleService(for car: Car) {
        // if car hasn't done presale services -> send it to cars array.
        if !car.isPresaleServiceDone {
            car.changePresaleStatus(to: true)
            cars.append(car)
            print("Car: \(car.model) has been added to all cars available list.")
        }
    }
    func addToShowroom(car: Car) throws {
            for (index, currentCar) in stockCars.enumerated() {
                // переганяем машину в автосалон на ней нет скидки
                if currentCar.serialNum == car.serialNum && showroomCars.count < showroomCapacity && !car.hasDiscount {
                    stockCars.remove(at: index)
                    self.presaleService(for: car)
                    showroomCars.append(car)
                    print("New car: \(car.model) has been added to showroom.")
                } else if showroomCars.count < showroomCapacity {
                    // Такой машины ещё нету, добавляем в гараж
                    self.presaleService(for: car)
                    showroomCars.append(car)
                    print("New car: \(car.model) has been added to showroom.")
                } else if car.hasDiscount && currentCar.serialNum == car.serialNum {
                    // В салоне уже есть такая машна, ещё одна не нужна там.
                    throw ShowRoomErrors.alreadyInShowroom
                }
            }
        }
    func sellCar(_ car: Car) {
        for (index, currentCar) in showroomCars.enumerated() {
            if currentCar.serialNum == car.serialNum {
                if currentCar.isPresaleServiceDone {
                    showroomCars.remove(at: index)
                    print("Car \(car.model) sold.")
                    if !currentCar.hasAdditionalGear {
                        print("I can offer you additional gear for your car, do you want to add it to the cart ?")
                    }
                } else if !car.isPresaleServiceDone {
                    self.presaleService(for: car)
                    showroomCars.remove(at: index)
                    print("Car \(car.model) sold.")
                }
            }
        }
    }
    func orderCar() {
        let models = ["Fusion", "Focus", "Mustang", "Mustang-gt"].randomElement() ?? "gazel"
        let randCar = CarModel(model: models, price: Int.random(in: 10_000...60_000), isPresaleServiceDone: false, hasAdditionalGear: false, year: Int.random(in: 2000...2024), hasDiscount: false)
        print("Ordered new car. Model: \(randCar.model), it's price \(randCar.price)$")
    }
}

class TeslaDealership: Dealership, SpecialOffer {
    let name: String
    let showroomCapacity: Int
    var stockCars = [Car]()
    var showroomCars = [Car]()
    var cars = [Car]()
    
    init(name: String, showroomCapacity: Int, stockCars: [Car], showroomCars: [Car], cars: [Car]) {
        self.name = name
        self.showroomCapacity = showroomCapacity
        self.stockCars = stockCars
        self.showroomCars = showroomCars
        self.cars = cars
    }
    
    func offerAccessories(from array: [Accessorie]) {
        // принимает массив акксесуаров в качестве параметра.
        // Метод предлагает клиенту купить доп. оборудование.
        let allAccessorieNames = array.map { $0.name }.joined(separator: ", ")
        print("We offer you some usefull accessories: \(allAccessorieNames)")
    }
    func presaleService(for car: Car) {
        // if car hasn't done presale services -> send it to cars array.
        if !car.isPresaleServiceDone {
            car.changePresaleStatus(to: true)
            cars.append(car)
            print("Car: \(car.model) has been added to all cars available list.")
        }
    }
    func addToShowroom(car: Car) throws {
            for (index, currentCar) in stockCars.enumerated() {
                // переганяем машину в автосалон на ней нет скидки
                if currentCar.serialNum == car.serialNum && showroomCars.count < showroomCapacity && !car.hasDiscount {
                    stockCars.remove(at: index)
                    self.presaleService(for: car)
                    showroomCars.append(car)
                    print("New car: \(car.model) has been added to showroom.")
                } else if showroomCars.count < showroomCapacity {
                    // Такой машины ещё нету, добавляем в гараж
                    self.presaleService(for: car)
                    showroomCars.append(car)
                    print("New car: \(car.model) has been added to showroom.")
                } else if car.hasDiscount && currentCar.serialNum == car.serialNum {
                    // В салоне уже есть такая машна, ещё одна не нужна там.
                    throw ShowRoomErrors.alreadyInShowroom
                }
            }
        }
    func sellCar(_ car: Car) {
        for (index, currentCar) in showroomCars.enumerated() {
            if currentCar.serialNum == car.serialNum {
                if currentCar.isPresaleServiceDone {
                    showroomCars.remove(at: index)
                    print("Car \(car.model) sold.")
                    if !currentCar.hasAdditionalGear {
                        print("I can offer you additional gear for your car, do you want to add it to the cart ?")
                    }
                } else if !car.isPresaleServiceDone {
                    self.presaleService(for: car)
                    showroomCars.remove(at: index)
                    print("Car \(car.model) sold.")
                }
            }
        }
    }
    func orderCar() {
        let models = ["Cyber-Truck", "S", "Y", "X"].randomElement() ?? "gazel"
        let randCar = CarModel(model: models, price: Int.random(in: 20_000...100_000), isPresaleServiceDone: false, hasAdditionalGear: false, year: Int.random(in: 2020...2024), hasDiscount: false)
        print("Ordered new car. Model: \(randCar.model), it's price \(randCar.price)$")
    }
}

struct Accessorie {
    let name: String
}

// Car park
let mercedesAmg63 = CarModel(model: "S63Amg", price: 120000, isPresaleServiceDone: true, hasAdditionalGear: false, year: 2024, hasDiscount: false)
let mercedesGwagon = CarModel(model: "G-wagon", price: 180000, isPresaleServiceDone: true, hasAdditionalGear: true, year: 2021, hasDiscount: true)
let teslaCyberTruck = CarModel(model: "Cyber-Truck", price: 80000, isPresaleServiceDone: true, hasAdditionalGear: true, year: 2024, hasDiscount: false)
let teslaModelS = CarModel(model: "Model-S", price: 95000, isPresaleServiceDone: true, hasAdditionalGear: false, year: 2023, hasDiscount: false)
let fordMustangGt = CarModel(model: "Mustang-gt", price: 40000, isPresaleServiceDone: false, hasAdditionalGear: false, year: 2023, hasDiscount: false)
let fordFusion = CarModel(model: "Fusion", price: 15000, isPresaleServiceDone: false, hasAdditionalGear: false, year: 2013, hasDiscount: true)

// Instances of Dealerships
let mercedesDealer = MercedesDealership(name: "MercedesBenz-Dealership", showroomCapacity: 230, stockCars: [mercedesAmg63], showroomCars: [ mercedesGwagon], cars: [mercedesAmg63, mercedesGwagon])
let fordDealer = FordDealership(name: "Ford-Dealership", showroomCapacity: 50, stockCars: [fordFusion], showroomCars: [fordMustangGt], cars: [fordMustangGt, fordFusion])
let teslaDealer = TeslaDealership(name: "Tesla-Dealership", showroomCapacity: 140, stockCars: [teslaModelS], showroomCars: [teslaCyberTruck], cars: [teslaCyberTruck, teslaModelS])

let allCarDealers: [AnyObject] = [mercedesDealer, fordDealer, teslaDealer]

// Accessories
let snowBrush = Accessorie(name: "Snowbrush")
let tirePump = Accessorie(name: "Tire-pump")

// Slogans
for car in allCarDealers {
    switch car {
    case let mercedesCarDealer as MercedesDealership:
        print("The best or nothing - \(mercedesCarDealer.name)")
    case let teslaCarDealer as TeslaDealership:
        print("Burn rubber, not gasoline - \(teslaCarDealer.name)")
    case let fordCarDealer as FordDealership:
        print("Build Ford Tough - \(fordCarDealer.name)")
    default: print("Unknown car")
    }
}

// Проверь все машины в дилерском центре (склад + автосалон), возможно они нуждаются в специальном предложении. Если есть машины со скидкой на складе, нужно перегнать их в автосалон.
// Задание⭐
mercedesDealer.showroomCars.forEach { car in
    if !car.hasDiscount {
        do {
            print("You car model - \(car.model)")
            try mercedesDealer.specialOffer(for: car)
        } catch SpecialOfferErrors.latestYear {
            print("Your car \(car.model) is the latest year of manufacture.")
        } catch {
            print("Unknown error.")
        }
    }
}

for (_, currentCar) in mercedesDealer.stockCars.enumerated() {
    if !currentCar.hasDiscount {
        do {
            try mercedesDealer.specialOffer(for: currentCar)
        } catch SpecialOfferErrors.latestYear {
            print("Your car \(currentCar.model) is the latest year of manufacture.")
        } catch {
            print("Unknown error.")
        }
    } else { // если есть скидка у машины из гаража
        do {
            try mercedesDealer.addToShowroom(car: currentCar)
        } catch ShowRoomErrors.alreadyInShowroom {
            print("This car \(currentCar.model) is already in Dealer's showroom with discount.")
        }
    }
}
