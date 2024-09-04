# DRY
```c
int checkTemperature(float temperature) {
    if (temperature < 0 || temperature > 45) {
        printf("Temperature out of range!\n");
        return 0;
    }
    return 1;
}

int checkSOC(float soc) {
    if (soc < 20 || soc > 80) {
        printf("State of Charge out of range!\n");
        return 0;
    }
    return 1;
}

int checkChargeRate(float chargeRate) {
    if (chargeRate > 0.8) {
        printf("Charge Rate out of range!\n");
        return 0;
    }
    return 1;
}
```

```C
bool isValueInRange(float minimum, float maximum, float value, const string& parameter)
{
  if (value < minimum || value > maximum)
  {
    printOutOfRange(parameter);
    return false;
  }
  return true;
}

bool isValueGreaterThan(float maximum, float value, const string& parameter)
{
  if (value > maximum)
  {
    printOutOfRange(parameter);
    return false;
  }
  return true;
}
```

# Bug Induced
```c
int batteryIsOk(float temperature, float soc, float chargeRate) 
{
  if(temperature < 0 || temperature > 45)
  {
    printf("Temperature out of range!\n");
    return 0;
  } 
  StateOfCharge(soc);
  ChargeRate(chargeRate);
  
  return 1;
}
```
```c
int batteryIsOk(float temperature, float soc, float chargeRate) {
    int temp_ok = !isTemperatureOk(temperature);
    int soc_ok = !isSocOk(soc);
    int charge_ok = !isChargeRateOk(chargeRate);
    
    disp_temp(temp_ok);
    disp_soc(soc_ok);
    disp_cr(charge_ok);
    
    return 1;
}
```

# Reusable Code With Assumptions
```c
int isParameterWithinLimits(float value, float min, float max, const char* message) {
    if (value < min || value > max) 
    {
        printf("%s\n", message);
        return 1;
    }
    return 0;
}
```
```c
int range_check(float value, float min, float max, const char* message) 
{    
    if (value < min || value > max) 
    {
        printf("%s\n", message);
        return 1;
    }
    
    return 0;
}
```
```c
int CheckInRange(float value, float min, float max, const char* parameterName)
{
  if(value < min || value > max) {
  printf("%s out of range bound/n",parameterName);
  return 0;
  }
return 1;
}
```
```c
int checkingInRange(float value, float min, float max, const char* parameterName){
   if(value<min || value>max){
       printf("%s out of range\n, parameterName);
       return 0;}
   return 1;
}
```
```c
bool checkRange(float value, float min, float max, const string& message) {
  if (value < min || value > max) {
    cout << message << endl;
    return false;
  }
  return true;
}
```
# Violation Of Principle of Least Knowledge
```c
int TempeartureSocIsOk(float temperature, float soc){
  if(!TemperatureIsOk(temperature)){
    return 0;
  }
  if(!socIsOk(soc)){
    return 0;
  }
  return 1;
}
```
# Conginitive Complexity Code
```c
int batteryIsOk(float temperature, float soc, float chargeRate) 
{
    return tempisok(temperature) && socisok(soc) && chrgRteisok(chargeRate);
}

```
```py
def battery_is_ok(temperature, soc, charge_rate, reporter=print):
    checks = [
        (temperature, 0, 45, 'Temperature'),
        (soc, 20, 80, 'State of Charge'),
        (charge_rate, 0, 0.8, 'Charge rate')
    ]
    
    return all(check_threshold(value, lower, upper, name, reporter) for value, lower, upper, name in checks)
```
```cs
  static bool isCRateOk(float cRate)
        {...}
 ```

# Allow Code to Express Reality
```c
// Function to verify minimum and maximum temperature
bool checkTemperature(float temperature)
// Function to verify minimum and maximum SOC value
bool checkSoc(float soc)
// Function to verify maximum charge rate
bool checkChargeRate(float chargeRate)

 // Temperature < 0, rest optimal
assert(batteryIsOk(-0.1, 70, 0.1) == false);
// Temperature > 45, rest optimal
assert(batteryIsOk(45.1, 70, 0.3) == false);
```

```py
TEMP_MIN = 0
"""Minimum acceptable temperature for the battery in degrees Celsius."""

TEMP_MAX = 45
"""Maximum acceptable temperature for the battery in degrees Celsius."""

SOC_MIN = 20
"""Minimum acceptable state of charge (SoC) for the battery in percentage."""

SOC_MAX = 80
"""Maximum acceptable state of charge (SoC) for the battery in percentage."""

CHARGE_RATE_MAX = 0.8
"""Maximum acceptable charge rate for the battery (0.8 or less)."""

```
```Py
# All values within range
    assert (battery_is_ok(25, 70, 0.7) is True)
    # Boundary values within range
    assert (battery_is_ok(0, 20, 0.8) is True)
    # Boundary values within range
    assert (battery_is_ok(45, 80, 0.8) is True)
    # Typical values within range
    assert (battery_is_ok(30, 50, 0.8) is True)
    # Typical values within range
    assert (battery_is_ok(25, 30, 0.5) is True)

    # Invalid scenarios
    # Temperature below minimum
    assert (battery_is_ok(-1, 70, 0.7) is False)
    # SoC below minimum
    assert (battery_is_ok(25, 19, 0.7) is False)
    # Charge rate above maximum
    assert (battery_is_ok(25, 70, 0.9) is False)
    # Temperature above maximum
    assert (battery_is_ok(46, 70, 0.7) is False)
    # SoC above maximum
    assert (battery_is_ok(25, 81, 0.7) is False)
    # Charge rate above maximum
    assert (battery_is_ok(25, 70, 0.81) is False)

    # Edge cases
    # Lower bounds of temperature, SoC, and charge rate
    assert (battery_is_ok(0, 20, 0.8) is True)
    # Upper bounds of temperature, SoC, and charge rate
    assert (battery_is_ok(45, 80, 0.8) is True)
    # SoC at upper bound with temperature at lower bound
    assert (battery_is_ok(0, 81, 0.8) is False)
    # Temperature at upper bound with SoC at lower bound
    assert (battery_is_ok(46, 20, 0.8) is False)
    # Charge rate at upper bound with valid temperature and SoC
    assert (battery_is_ok(25, 80, 0.81) is False)
```
```py
 assert(battery_is_ok(25, 70, 0.7) is True)
    assert(battery_is_ok(50, 85, 0) is False)  # High temperature, high soc, low charge rate
    assert(battery_is_ok(-5, 70, 0.7) is False)  # Low temperature
    assert(battery_is_ok(25, 15, 0.7) is False)  # Low State of Charge
    assert(battery_is_ok(25, 70, 0.9) is False)  # High charge rate
    assert(battery_is_ok(50, 70, 0.7, custom_reporter) is False)  # Using a custom reporter
```
# Side Effect ?
```c
#define OK 1
#define NOT_OK 0
#define TEMP_MIN 0
#define TEMP_MAX 45
#define SOC_MIN 20
#define SOC_MAX 80
#define CHARGE_RATE 0.8


bool isTemperatureOk(float temperature)
{
    return (temperature >= TEMP_MIN  && temperature <= TEMP_MAX);
}

bool isSocOk(float soc)
{
    return (soc >= SOC_MIN && soc <= SOC_MAX);
}

bool isChargeRateOk(float chargeRate)
{
   return (chargeRate <= CHARGE_RATE);
}
```

# Multi Paradigm Solution (Good Design  - "As of Now")
```c++
enum class BreachType { NORMAL, TOO_LOW, TOO_HIGH };

class BatteryParameter {
public:
    string name;
    float minLimit;
    float maxLimit;

    BatteryParameter(string paramName, float min, float max)
        : name(paramName), minLimit(min), maxLimit(max) {}
};

BreachType classifyBreach(float value, const BatteryParameter& parameter) {
    if (value < parameter.minLimit) {
        return BreachType::TOO_LOW;
    }
    if (value > parameter.maxLimit) {
        return BreachType::TOO_HIGH;
    }
    return BreachType::NORMAL;
}
void printBreachMessage(const BatteryParameter& parameter, BreachType breachType) {
    if (breachType == BreachType::TOO_LOW) {
        cout << parameter.name << " too low!\n";
    } else if (breachType == BreachType::TOO_HIGH) {
        cout << parameter.name << " too high!\n";
    }
}

bool checkAndReportBreach(float value, const BatteryParameter& parameter) {
    BreachType breachType = classifyBreach(value, parameter);
    if (breachType != BreachType::NORMAL) {
        printBreachMessage(parameter, breachType);
        return false;
    }
    return true;
}



bool batteryIsOk(float temperature, float soc, float chargeRate) {
    BatteryParameter temperatureParam("Temperature", 0, 45);
    BatteryParameter socParam("State of Charge", 20, 80);
    BatteryParameter chargeRateParam("Charge Rate", 0, 0.8);

    bool temperatureOk = checkAndReportBreach(temperature, temperatureParam);
    bool socOk = checkAndReportBreach(soc, socParam);
    bool chargeRateOk = checkAndReportBreach(chargeRate, chargeRateParam);

    return temperatureOk && socOk && chargeRateOk;
}

void runTests() {
    assert(batteryIsOk(25, 70, 0.7) == true);
    assert(batteryIsOk(-5, 70, 0.7) == false);
    assert(batteryIsOk(25, 85, 0.7) == false);
    assert(batteryIsOk(25, 70, 0.9) == false);
    assert(batteryIsOk(50, 85, 0.9) == false);
}
```
- https://github.com/tts-tcq-2024/paradigm-shift-in-cpp-AnkitRawat1710.git

# Pure Functions in Action
```c++

bool isTemperatureOk(float temperature)
{
  return(temperature >= 0 && temperature <= 45);
}

bool isSocOk(float soc)
{
  return(soc >=20 && soc <=80);
}

bool isChargeRateOk(float chargeRate)
{
  return(chargeRate <= 0.8);
}
```

```py
def is_within_range(value, min_value, max_value):
    """Check if a value is within the specified range.
    Args:
        value (float): The value to check.
        min_value (float): The minimum acceptable value (inclusive).
        max_value (float): The maximum acceptable value (inclusive).

    Returns:
        bool: True if the value is within the range [min_value, max_value];
        False otherwise.
    """
    return min_value <= value <= max_value
```
## Optimistic Code (Be Pesimistic)
```c
bool batteryIsOk(float temperature, float soc, float chargeRate) {
    bool isOk = true; // Assume battery is okay by default

    // Check temperature
    if (temperature < 0 || temperature > 45) {
        cout << "Temperature out of range!\n";
        isOk = false;
    }
    
    // Check state of charge
    if (soc < 20 || soc > 80) {
        cout << "State of Charge out of range!\n";
        isOk = false;
    }
    
    // Check charge rate
    if (chargeRate > 0.8) {
        cout << "Charge Rate out of range!\n";
        isOk = false;
    }

    return isOk; // Return the final status
}

```

