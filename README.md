# Swinject

[![Travis CI](https://travis-ci.org/Swinject/Swinject.svg?branch=master)](https://travis-ci.org/Swinject/Swinject)
[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)
[![CocoaPods Version](https://img.shields.io/cocoapods/v/Swinject.svg?style=flat)](http://cocoapods.org/pods/Swinject)
[![License](https://img.shields.io/cocoapods/l/Swinject.svg?style=flat)](http://cocoapods.org/pods/Swinject)
[![Platform](https://img.shields.io/cocoapods/p/Swinject.svg?style=flat)](http://cocoapods.org/pods/Swinject)
[![Language Swift](https://img.shields.io/badge/language-Swift-F16D39.svg?style=flat)](https://developer.apple.com/swift)

Swinject is a lightweight [dependency injection](https://en.wikipedia.org/wiki/Dependency_injection) framework for Swift, inspired by [Ninject](http://ninject.org), [Autofac](http://autofac.org), [Typhoon](http://typhoonframework.org), and highly inspired by [Funq](http://funq.codeplex.com).

Dependency injection (DI) is a software design pattern that implements Inversion of Control (IoC) for resolving dependencies. In the pattern, Swinject helps your app split into loosely-coupled components, which can be developed, tested and maintained more easily. Swinject is powered by the Swift generic type system and first class functions to define dependencies of your app simply and fluently.

## Features

- [x] Pure Swift Type Injection
- [x] Initializer/Property/Method Injections
- [x] Initialization Callback
- [x] Circular Dependency Injection
- [x] Injection with Arguments
- [x] Self-registration (Self-binding)
- [x] Container Hierarchy
- [x] Object Scopes as None (Transient), Graph, Container (Singleton) and Hierarchy
- [x] Injection of both Reference and Value Types
- [x] Storyboard

## Requirements

- iOS 8.0+ / Mac OS X 10.10+
- Xcode 7.0

## Installation

Swinject is available through [Carthage](https://github.com/Carthage/Carthage) or  [CocoaPods](https://cocoapods.org).

### Carthage

To install Swinject with Carthage, add the following line to your `Cartfile`.

    github "Swinject/Swinject" ~> 0.2


Then run `carthage update` command. For details of the installation and usage of Carthage, visit [its project page](https://github.com/Carthage/Carthage).


### CocoaPods

To install Swinject with CocoaPods, add the following lines to your `Podfile`.

    source 'https://github.com/CocoaPods/Specs.git'
    platform :ios, '8.0'
    use_frameworks!

    pod 'Swinject', '~> 0.2'

Then run `pod install` command. For details of the installation and usage of CocoaPods, visit [its official website](https://cocoapods.org).

## Documentation

All documentation can be found in the [Documentation folder](./Documentation), including patterns of dependency injection and examples.

## Basic Usage

First, register a service and component pair to a `Container`, where the component is created by the registered closure as a factory. In this example, `Cat` and `PetOwner` are component classes implementing `AnimalType` and `PersonType` service protocols, respectively.

    let container = Container()
    container.register(AnimalType.self) { _ in Cat(name: "Mimi") }
    container.register(PersonType.self) { r in
         PetOwner(pet: r.resolve(AnimalType.self)!)
    }

Then get an instance of a service from the container. The person is resolved to a pet owner, and playing with the cat named Mimi!

    let person = container.resolve(PersonType.self)!
    person.play() // prints "I'm playing with Mimi."

Where definitions of the protocols and classes are

    protocol AnimalType {
        var name: String? { get }
    }

    class Cat: AnimalType {
        let name: String?

        init(name: String?) {
            self.name = name
        }
    }

and

    protocol PersonType {
        func play()
    }

    class PetOwner: PersonType {
        let pet: AnimalType

        init(pet: AnimalType) {
            self.pet = pet
        }

        func play() {
            let name = pet.name ?? "someone"
            print("I'm playing with \(name).")
        }
    }

Notice that the `pet` of `PetOwner` is automatically set as the instance of `Cat` when `PersonType` is resolved to the instance of `PetOwner`. If a container already set up is given, you do not have to care what are the actual types of the services and how they are created with their dependency.

## Play in Playground!

The project contains `Sample-iOS.playground` to demonstrate the features of Swinject. Download or clone the project, run the playground, modify it, and play with it to learn Swinject.

To run the playground in the project, first build the project, then select `Editor > Execute Playground` menu in Xcode.

## Example Apps

- [SwinjectSimpleExample](https://github.com/Swinject/SwinjectSimpleExample) demonstrates dependency injection and Swinject in a simple weather app that lists current weather information at some locations.
- [SwinjectMVVMExample](https://github.com/Swinject/SwinjectMVVMExample) demonstrates dependency injection with Swift and reactive programming with [ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa) in MVVM architecture.

## Blog Posts

The following blog posts introduce Swinject and the concept of dependency injection.

- [Dependency Injection Framework for Swift - Introduction to Swinject](https://yoichitgy.github.io/post/dependency-injection-framework-for-swift-introduction-to-swinject/)
- [Dependency Injection Framework for Swift - Simple Weather App Example with Swinject Part 1/2](https://yoichitgy.github.io/post/dependency-injection-framework-for-swift-simple-weather-app-example-with-swinject-part-1/)
- [Dependency Injection Framework for Swift - Simple Weather App Example with Swinject Part 2/2](https://yoichitgy.github.io/post/dependency-injection-framework-for-swift-simple-weather-app-example-with-swinject-part-2/)

## License

MIT license. See the [LICENSE file](LICENSE.txt) for details.
