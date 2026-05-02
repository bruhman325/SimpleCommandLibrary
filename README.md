# SimpleCommandLibrary (SCL)
Simple Command Library is a library that aids in making simple command line tools.

## Installation
To use this library simply clone this repo and put in somewhere in your project then link it to your project using cmake

### Example (CMakeLists.txt): 
```
project(ProjectName)

add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/thirdparty/SimpleCommandLibrary)

add_executable(executablename)

target_sources(executablename PRIVATE main.cpp)

target_link_libraries(executablename PRIVATE SCL)


```


## Getting Started

### Step one:
Create a SCL::CommandManager object in your main function and and send the argc and argv arguments to manager.HandleCommand();
```
int main(int argc, char** argv) {
    SCL::CommandManager manager = {};

    manager.HandleCommand(argc,argv);
    return 0;
}

```

### Step two:
Create a callback function for your command

```
void ExampleFunction(SCL::UserInput input, SCL::CommandManager manager, std::shared_ptr<void> data) {
    std::cout << "hello world";
}
```

### Step three:
Register your command 

```
int main(int argc, char** argv) {
    SCL::CommandManager manager = {};
    manager.RegisterCommand("name","description", ExampleFunction, NULL);
    manager.HandleCommand(argc,argv);
    return 0;
}
```

### Step four:
build and run with whatever you named your command

## More Info:
### SCL::UserInput


```
  This class consists of: 
  +vector<string> args // anything that is not a flag (see below)
  +vector<string> flags // anything that the user sent in with -- prefixing it (ex. --clear)
```

### Data
Data from the main function to the command function is sent via a shared_ptr. <br> Example:

```

void ExampleFunction(SCL::UserInput input, SCL::CommandManager manager, std::shared_ptr<void> data) {
	std::shared_ptr<int> convertedData = std::static_pointer_cast<int>(data);
	std::cout << *convertedData;
}

int main(int argc, char** argv) {
    SCL::CommandManager manager = {};
    std::shared_ptr<int> data = std::make_shared<int>(3);
    manager.RegisterCommand("name","description", ExampleFunction, data);
    manager.HandleCommand(argc,argv);
    return 0;
}

```


### SCL::CommandManager::RegisterDefaultBehavior
Use this if you want to create functionality like 
```
program_name arg1 arg2 arg3  
```

instead of 

```
program_name command arg1 arg2 arg3
```

It will send any invalid commands to the default behavior so it doesnt have to be mutually exclusive
