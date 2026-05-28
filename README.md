# Spring Boot Todo App

A simple and elegant Todo application built with Spring Boot and HTML. This application allows users to create, read, update, and delete todo items with an intuitive web interface.

## Features

- ✅ Create new todo items
- ✅ View all todos
- ✅ Mark todos as complete/incomplete
- ✅ Update existing todos
- ✅ Delete todos
- ✅ Clean and responsive user interface
- ✅ Lightweight and fast performance

## Technology Stack

- **Backend**: Java, Spring Boot
- **Frontend**: HTML, CSS, JavaScript
- **Database**: (Configure based on your setup - H2, MySQL, PostgreSQL, etc.)
- **Build Tool**: Maven/Gradle

## Prerequisites

Before you begin, ensure you have the following installed:

- Java Development Kit (JDK) 11 or higher
- Maven 3.6.0 or higher (or Gradle 6.0+)
- Git

## Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/RahulPawar178/spring-boot-todo-app.git
   cd spring-boot-todo-app
   ```

2. **Build the project**
   ```bash
   mvn clean install
   ```

3. **Run the application**
   ```bash
   mvn spring-boot:run
   ```

4. **Access the application**
   Open your browser and navigate to:
   ```
   http://localhost:8080
   ```

## Project Structure

```
spring-boot-todo-app/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── (Java source files)
│   │   └── resources/
│   │       ├── templates/
│   │       └── application.properties
│   └── test/
├── pom.xml
└── README.md
```

## Usage

1. **Add a Todo**: Enter your todo item in the input field and click "Add"
2. **Complete a Todo**: Check the checkbox next to the todo to mark it as complete
3. **Edit a Todo**: Click the edit button to modify an existing todo
4. **Delete a Todo**: Click the delete button to remove a todo from the list

## Configuration

You can customize the application by modifying `src/main/resources/application.properties`:

```properties
spring.application.name=spring-boot-todo-app
server.port=8080
```

## API Endpoints (if applicable)

- `GET /todos` - Retrieve all todos
- `POST /todos` - Create a new todo
- `PUT /todos/{id}` - Update a todo
- `DELETE /todos/{id}` - Delete a todo

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a new branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

This project is open source and available under the MIT License.

## Support

If you encounter any issues or have questions, please open an [issue](https://github.com/RahulPawar178/spring-boot-todo-app/issues) on the repository.

## Acknowledgments

- Spring Boot Documentation
- Community contributors

---

**Happy coding!** 🚀
