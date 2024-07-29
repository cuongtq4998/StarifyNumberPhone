# StarifyNumberPhone

Main class: init implement class
```
class MyPlayground {
    func run(format: NumberPhoneFormater) -> String {
        return format.format(character: "*")
    }
}

let myPhone = "0329339111"
let main = MyPlayground()

let stari = MidleNumberPhoneFormater(myPhone: myPhone)
let stariFormat = main.run(format: stari)
print(stariFormat)

let mask = MaskNumberPhoneFormater(myPhone: myPhone)
let maskFormat = main.run(format: mask)
print(maskFormat)
```

interface, implement class

```
protocol NumberPhoneFormater {
    var myPhone: String { get }
    func format(character: String) -> String
}

struct MidleNumberPhoneFormater: NumberPhoneFormater {
    let myPhone: String
    
    func format(character: String) -> String {
        print(myPhone)
        
        guard myPhone.isValidPhoneVN() else {
            print("throw: \(myPhone)")
            return myPhone
        }
        
        let maskedPhone = myPhone.starifyNumberPhone()
        print(maskedPhone)
        return maskedPhone
    }
}

struct MaskNumberPhoneFormater: NumberPhoneFormater {
    let myPhone: String
    
    func format(character: String) -> String {
        print(myPhone)
        
        guard myPhone.isValidPhoneVN() else {
            print("throw: \(myPhone)")
            return myPhone
        }
        
        let maskedPhone = myPhone.masked(2, reversed: true)
        print(maskedPhone)
        return maskedPhone
    }
}
```

String Extension
```
public extension StringProtocol {
    func masked(_ n: Int = 5, reversed: Bool = false) -> String {
        let mask = String(repeating: "*", count: Swift.max(0, count-n))
        return reversed ? mask + suffix(n) : prefix(n) + mask
    }
}

extension String {
    func isValidPhoneVN() -> Bool {
        let phonePattern = #"([\+84|84|0]+(3|5|7|8|9|1[2|6|8|9]))+([0-9]{8})"#
        let result = self.range(
            of: phonePattern,
            options: .regularExpression
        )
        return (result != nil)
    }
    
    func starifyNumberPhone(character: String = "*") -> String {
        let intLetters = self.prefix(3)
        let endLetters = self.suffix(4)
        let numberOfStars = self.count - (intLetters.count + endLetters.count)
        var starString = ""
        for _ in 1...numberOfStars {
            starString += character
        }
        let finalNumberToShow: String = intLetters + starString + endLetters
        return finalNumberToShow
    }
}
```

