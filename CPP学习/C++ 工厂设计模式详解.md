C++ 中的工厂设计模式是一种常用的创建型设计模式，它旨在**将对象的创建与使用分离**，从而提升代码的灵活性、可维护性和可扩展性。下面我将由浅入深地为你介绍三种主要的工厂模式：简单工厂模式、工厂方法模式和抽象工厂模式。

# 🏭 C++ 工厂设计模式详解

## 1️⃣ 简单工厂模式

简单工厂模式通过一个单独的工厂类来封装对象的创建过程，根据传入的参数决定创建哪种产品类的实例。

### 实现方式

- **工厂类**：包含一个静态方法，根据输入参数创建并返回具体产品对象。
- **抽象产品类**：定义所有具体产品的公共接口。
- **具体产品类**：实现抽象产品接口，是工厂要创建的目标。

```
#include <iostream>
#include <memory>

// 抽象产品类
class Product {
public:
    virtual ~Product() = default;
    virtual void use() const = 0;
};

// 具体产品A
class ConcreteProductA : public Product {
public:
    void use() const override {
        std::cout << "Using Product A\n";
    }
};

// 具体产品B
class ConcreteProductB : public Product {
public:
    void use() const override {
        std::cout << "Using Product B\n";
    }
};

// 简单工厂
class SimpleFactory {
public:
    enum ProductType { A, B };
    
    static std::unique_ptr<Product> createProduct(ProductType type) {
        switch (type) {
            case A: return std::make_unique<ConcreteProductA>();
            case B: return std::make_unique<ConcreteProductB>();
            default: return nullptr;
        }
    }
};

int main() {
    auto productA = SimpleFactory::createProduct(SimpleFactory::A);
    productA->use();  // 输出: Using Product A
    
    auto productB = SimpleFactory::createProduct(SimpleFactory::B);
    productB->use();  // 输出: Using Product B
    
    return 0;
}
```

### 优缺点

- **优点**：将对象创建封装在工厂中，客户端无需关心创建细节。
- **缺点**：违反开闭原则，新增产品类型时需要修改工厂类的逻辑。

## 2️⃣ 工厂方法模式

工厂方法模式通过引入抽象工厂类和具体工厂子类来解决简单工厂模式的问题，将对象的创建延迟到子类进行。

### 实现方式

- **抽象工厂类**：声明创建产品的工厂方法。
- **具体工厂类**：实现抽象工厂接口，负责创建具体产品对象。

```
#include <iostream>
#include <memory>

// 抽象产品类
class Product {
public:
    virtual ~Product() = default;
    virtual void operation() = 0;
};

// 具体产品A
class ConcreteProductA : public Product {
public:
    void operation() override {
        std::cout << "ConcreteProductA operation" << std::endl;
    }
};

// 具体产品B
class ConcreteProductB : public Product {
public:
    void operation() override {
        std::cout << "ConcreteProductB operation" << std::endl;
    }
};

// 抽象工厂
class Factory {
public:
    virtual ~Factory() = default;
    virtual std::unique_ptr<Product> createProduct() const = 0;
};

// 具体工厂A
class ConcreteFactoryA : public Factory {
public:
    std::unique_ptr<Product> createProduct() const override {
        return std::make_unique<ConcreteProductA>();
    }
};

// 具体工厂B
class ConcreteFactoryB : public Factory {
public:
    std::unique_ptr<Product> createProduct() const override {
        return std::make_unique<ConcreteProductB>();
    }
};

int main() {
    std::unique_ptr<Factory> factoryA = std::make_unique<ConcreteFactoryA>();
    auto productA = factoryA->createProduct();
    productA->operation();  // 输出: ConcreteProductA operation
    
    std::unique_ptr<Factory> factoryB = std::make_unique<ConcreteFactoryB>();
    auto productB = factoryB->createProduct();
    productB->operation();  // 输出: ConcreteProductB operation
    
    return 0;
}
```

### 优缺点

- **优点**：符合开闭原则，新增产品时只需添加新的工厂类，无需修改现有代码。
- **缺点**：每增加一个产品就需要增加一个具体工厂类，会导致系统类数量增多。

## 3️⃣ 抽象工厂模式

抽象工厂模式提供一个接口用于创建相关或依赖对象的家族，而无需指定它们的具体类。

### 实现方式

- **抽象工厂类**：声明一系列创建产品的方法。
- **具体工厂类**：实现抽象工厂接口，生产同一产品族的不同产品。
- **抽象产品类**：定义产品的接口。
- **具体产品类**：实现抽象产品接口，是工厂要创建的目标。

```
#include <iostream>
#include <memory>

// 抽象产品A
class AbstractProductA {
public:
    virtual ~AbstractProductA() = default;
    virtual void operationA() const = 0;
};

// 具体产品A1
class ProductA1 : public AbstractProductA {
public:
    void operationA() const override {
        std::cout << "ProductA1 operationA" << std::endl;
    }
};

// 具体产品A2
class ProductA2 : public AbstractProductA {
public:
    void operationA() const override {
        std::cout << "ProductA2 operationA" << std::endl;
    }
};

// 抽象产品B
class AbstractProductB {
public:
    virtual ~AbstractProductB() = default;
    virtual void operationB() const = 0;
};

// 具体产品B1
class ProductB1 : public AbstractProductB {
public:
    void operationB() const override {
        std::cout << "ProductB1 operationB" << std::endl;
    }
};

// 具体产品B2
class AbstractProductB {
public:
    void operationB() const override {
        std::cout << "ProductB2 operationB" << std::endl;
    }
};

// 抽象工厂
class AbstractFactory {
public:
    virtual ~AbstractFactory() = default;
    virtual std::unique_ptr<AbstractProductA> createProductA() const = 0;
    virtual std::unique_ptr<AbstractProductB> createProductB() const = 0;
};

// 具体工厂1
class ConcreteFactory1 : public AbstractFactory {
public:
    std::unique_ptr<AbstractProductA> createProductA() const override {
        return std::make_unique<ProductA1>();
    }
    
    std::unique_ptr<AbstractProductB> createProductB() const override {
        return std::make_unique<ProductB1>();
    }
};

// 具体工厂2
class ConcreteFactory2 : public AbstractFactory {
public:
    std::unique_ptr<AbstractProductA> createProductA() const override {
        return std::make_unique<ProductA2>();
    }
    
    std::unique_ptr<AbstractProductB> createProductB() const override {
        return std::make_unique<ProductB2>();
    }
};

int main() {
    std::unique_ptr<AbstractFactory> factory1 = std::make_unique<ConcreteFactory1>();
    auto productA1 = factory1->createProductA();
    auto productB1 = factory1->createProductB();
    productA1->operationA();  // 输出: ProductA1 operationA
    productB1->operationB();  // 输出: ProductB1 operationB
    
    std::unique_ptr<AbstractFactory> factory2 = std::make_unique<ConcreteFactory2>();
    auto productA2 = factory2->createProductA();
    auto productB2 = factory2->createProductB();
    productA2->operationA();  // 输出: ProductA2 operationA
    productB2->operationB();  // 输出: ProductB2 operationB
    
    return 0;
}
```

### 优缺点

- **优点**：保证产品族的一致性，易于交换产品系列。
- **缺点**：难以支持新种类的产品，因为需要修改抽象工厂接口及其所有子类。

## 📊 三种工厂模式对比

| **特性**         | **简单工厂模式**     | **工厂方法模式**     | **抽象工厂模式**                   |
| ---------------- | -------------------- | -------------------- | ---------------------------------- |
| **设计复杂度**   | 低                   | 中                   | 高                                 |
| **可扩展性**     | 差（需修改工厂类）   | 好（增加新工厂）     | 较好（增加新产品族容易，新产品难） |
| **符合开闭原则** | 否                   | 是                   | 是（针对产品族）                   |
| **适用场景**     | 产品类型少且不常变化 | 产品类型多且可能扩展 | 产品族多个且需要保持一致           |

## 5️⃣ 总结与应用场景

- **简单工厂模式**：适用于产品类型较少且不常变化的简单场景，虽然不符合开闭原则，但实现简单。
- **工厂方法模式**：适用于产品类型多且可能需要扩展的场景，符合开闭原则，是真正意义上的标准工厂模式。
- **抽象工厂模式**：适用于需要创建一系列相关或依赖对象的场景，强调产品族的概念和一致性。

工厂模式通过多态特性将存在继承关系的类，通过一个工厂类创建对应的子类对象，在项目复杂的情况下，可以便于子类对象的创建，实现创建和使用的分离，降低代码的重复率。

希望以上解释能帮助你理解C++中的工厂设计模式。根据你的具体需求，你可以选择最适合的模式。