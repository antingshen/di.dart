## Note
This is a read-only copy of the DI library now over at [Angular](https://github.com/angular/di.dart).  
For my 2.0 overhaul, see [commit 10e471](https://github.com/antingshen/di.dart/commit/10e471b0ea2e4392d436cfa555f6c613fc4a7043).

# Dependency Injection (DI) framework

## Installation

Add dependency to your pubspec.yaml.

    dependencies:
      di: ">=2.0.0 <3.0.0"

Then, run `pub install`.

Import di.

    import 'package:di/di.dart';

## Example

```dart
import 'package:di/di.dart';

abstract class Engine {
  go();
}

class Fuel {}

class V8Engine implements Engine {
  Fuel fuel;
  V8Engine(this.fuel);
  
  go() {
    print('Vroom...');
  }
}

class ElectricEngine implements Engine {
  go() {
    print('Hum...');
  }
}

// Annotation
class Electric {
  const Electric();
}

class GenericCar {
  Engine engine;

  GenericCar(this.engine);

  drive() {
    engine.go();
  }
}

class ElectricCar {
  Engine engine;

  ElectricCar(@Electric() this.engine);

  drive() {
    engine.go();
  }
}

void main() {
  var injector = new ModuleInjector(modules: [new Module()
      ..bind(GenericCar)
      ..bind(ElectricCar)
      ..bind(Engine, toFactory: (fuel) => new V8Engine(fuel), inject: [Fuel])
      ..bind(Engine, toImplementation: ElectricEngine, withAnnotation: Electric)
  ]);
  injector.get(GenericCar).drive(); // Vroom...
  injector.get(ElectricCar).drive(); // Hum...
}
```
