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
```C++
bool batteryIsOk(float temperature, float soc, float chargeRate) 
{
  bool tempOk = isTemperatureOk(temperature);
  bool socOk = isSocOk(soc);
  bool chargeRateOk = isChargeRateOk(chargeRate);

  if (tempOk && socOk && chargeRateOk)
  {
    return true;
  }    

  if(!tempOk)
  {
    cout << "Temperature out of range!" << endl;
  }

  if(!socOk)
  {
    cout << "State of Charge out of range!" << endl;
  }

  if(!chargeRateOk)
  {
    cout << "Charge rate out of range!" << endl;
  }
  
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
