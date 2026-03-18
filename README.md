# ImageGallery

A full-stack image gallery application built with ASP.NET Core and React that enables users to upload, view, and manage images with an intuitive interface and robust backend.

**Live Application:** https://imagegallery.paweldywandev.com/

## Overview

ImageGallery is a modern web application that demonstrates a clean separation of concerns using a multi-layered architecture. The backend is built with ASP.NET Core 8.0 and follows best practices for RESTful API design, while the frontend leverages React with TypeScript for type-safe, component-based development. The application uses PostgreSQL for data persistence and includes automated database migrations and seeding capabilities.

## Key Features

- **Image Upload**: Upload images with optional title and description metadata
- **Image Gallery**: View all uploaded images in a responsive grid layout
- **Image Management**: Delete images with automatic cleanup of both database records and physical files
- **RESTful API**: Well-structured API endpoints following REST conventions
- **Database Migrations**: Entity Framework Core migrations for version-controlled database schema
- **Data Seeding**: Automatic database initialization and sample data seeding
- **Swagger Documentation**: Interactive API documentation available in development mode
- **Static File Serving**: Efficient serving of uploaded images through ASP.NET Core middleware
- **SPA Integration**: Seamless integration between backend API and React frontend
- **Type Safety**: Full TypeScript support in the frontend for enhanced developer experience

## Architecture

The solution follows a clean, layered architecture pattern with clear separation of concerns:

```
ImageGallery/
|
+-- ImageGallery.Server/          # ASP.NET Core Web API
|   +-- Controllers/               # API endpoints
|   +-- Program.cs                 # Application entry point and configuration
|   +-- appsettings.json           # Application settings
|
+-- ImageGallery.BLL/              # Business Logic Layer
|   +-- Interfaces/                # Service contracts
|   +-- Services/                  # Business logic implementation
|   +-- Models/                    # Data transfer objects (DTOs)
|   +-- ImageGalleryMappingProfile.cs  # AutoMapper configuration
|
+-- ImageGallery.DAL/              # Data Access Layer
|   +-- Entities/                  # Entity models
|   +-- Migrations/                # EF Core migrations
|   +-- ImageGalleryContext.cs     # Database context
|   +-- ImageGallerySeeder.cs      # Database seeding logic
|
+-- imagegallery.client/           # React Frontend
    +-- src/                       # Source files
    +-- package.json               # Node dependencies
    +-- vite.config.ts             # Vite configuration

```

### Layer Responsibilities

**ImageGallery.Server**: Houses the Web API controllers and application startup configuration. Responsible for HTTP request handling, middleware pipeline setup, dependency injection configuration, and serving the React SPA.

**ImageGallery.BLL**: Contains business logic, service interfaces, and DTOs. Implements AutoMapper for object-to-object mapping and handles business rules and validation.

**ImageGallery.DAL**: Manages data persistence with Entity Framework Core. Includes entity definitions, database context, migrations, and seeding logic for PostgreSQL database.

**imagegallery.client**: React-based frontend built with Vite, featuring TypeScript for type safety, Bootstrap/Reactstrap for UI components, and modern React patterns.

## Getting Started

### Prerequisites

Before running the application, ensure you have the following installed:

- **.NET 8 SDK** - Download from [https://dotnet.microsoft.com/download](https://dotnet.microsoft.com/download)
- **Node.js (v18 or later)** and **npm** - Download from [https://nodejs.org/](https://nodejs.org/)
- **PostgreSQL (v12 or later)** - Download from [https://www.postgresql.org/download/](https://www.postgresql.org/download/)
- **Git** - Download from [https://git-scm.com/downloads](https://git-scm.com/downloads)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/paweldywandev/ImageGallery.git
   cd ImageGallery
   ```

2. **Configure the database connection**
   
   Create or edit `ImageGallery.Server/appsettings.Development.json` and add your PostgreSQL connection string:
   ```json
   {
     "ConnectionStrings": {
       "DefaultConnection": "Host=localhost;Database=imagegallery;Username=your_username;Password=your_password"
     }
   }
   ```

3. **Install backend dependencies**
   ```bash
   cd ImageGallery.Server
   dotnet restore
   ```

4. **Apply database migrations**
   
   The application will automatically run migrations on startup, but you can also run them manually:
   ```bash
   dotnet ef database update
   ```

5. **Install frontend dependencies**
   ```bash
   cd ../imagegallery.client
   npm install
   ```

### Running the Application

#### Development Mode

1. **Start the backend server**
   ```bash
   cd ImageGallery.Server
   dotnet run
   ```
   The API will be available at `https://localhost:5001` (or as configured in `launchSettings.json`)
   Swagger UI will be accessible at `https://localhost:5001/swagger`

2. **Start the frontend development server**
   
   In a new terminal window:
   ```bash
   cd imagegallery.client
   npm run dev
   ```
   The React app will be available at `https://localhost:5173`

#### Production Build

1. **Build the frontend**
   ```bash
   cd imagegallery.client
   npm run build
   ```

2. **Run the backend** (which will serve the built frontend)
   ```bash
   cd ../ImageGallery.Server
   dotnet run --configuration Release
   ```

## Technology Stack

### Backend

- **ASP.NET Core 8.0** - Web framework for building the API
- **Entity Framework Core 8.0** - ORM for database access
- **Npgsql.EntityFrameworkCore.PostgreSQL 8.0.10** - PostgreSQL provider for EF Core
- **AutoMapper 13.0.1** - Object-to-object mapping
- **Swashbuckle.AspNetCore 6.9.0** - Swagger/OpenAPI documentation

### Frontend

- **React 18.3.1** - UI library for building user interfaces
- **TypeScript 5.6.3** - Type-safe JavaScript
- **Vite 5.4.10** - Build tool and development server
- **Bootstrap 5.3.3** - CSS framework
- **Reactstrap 9.2.3** - React components for Bootstrap
- **ESLint 9.13.0** - Code linting and quality

### Database

- **PostgreSQL** - Relational database for storing image metadata

## API Documentation

The API provides the following endpoints:

### Get All Images
```
GET /api/image
```
Returns a list of all images with their metadata.

**Response:** `200 OK`
```json
[
  {
    "id": 1,
    "fileName": "example",
    "extension": ".jpg",
    "title": "Example Image",
    "description": "This is an example image",
    "url": "/Images/1.jpg"
  }
]
```

### Upload Image
```
POST /api/image
```
Uploads a new image with optional metadata.

**Request:** `multipart/form-data`
- `file` (required): Image file
- `fileName` (required): Original file name
- `extension` (required): File extension
- `title` (optional): Image title
- `description` (optional): Image description

**Response:** `200 OK`

### Delete Image
```
DELETE /api/image/{id}
```
Deletes an image by ID, removing both the database record and physical file.

**Parameters:**
- `id` (path): Image ID

**Response:** `200 OK`

### Swagger UI

Interactive API documentation is available in development mode at:
```
https://localhost:5001/swagger
```

## Project Configuration

### Database Seeding

The application automatically runs database migrations and seeding on startup through the `ImageGallerySeeder` class. The seeder is invoked in `Program.cs` to ensure the database is properly initialized.

### Static File Serving

Uploaded images are stored in the `Images` directory within the server's working directory. The application configures static file middleware to serve these images at the `/Images` path.

### CORS and Security

Configure CORS policies in `Program.cs` if you need to allow cross-origin requests from specific domains. Update the `AllowedHosts` setting in `appsettings.json` for production deployments.

## Contributing

Contributions are welcome! Please follow these guidelines:

1. **Fork the repository**
   ```bash
   git clone https://github.com/paweldywandev/ImageGallery.git
   cd ImageGallery
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes**
   - Follow the existing code style and conventions
   - Ensure all projects build successfully
   - Test your changes thoroughly

3. **Commit your changes**
   ```bash
   git add .
   git commit -m "Add your descriptive commit message"
   ```

4. **Push to your fork and submit a pull request**
   ```bash
   git push origin feature/your-feature-name
   ```

### Code Style Guidelines

- Use meaningful variable and method names
- Follow C# and TypeScript naming conventions
- Add XML documentation comments for public APIs
- Keep methods focused and single-purpose
- Write clean, readable code

## License

This project is licensed under the MIT License. Feel free to use, modify, and distribute this software as per the license terms.

## Author

**Pawel Dywan**

- GitHub: [@paweldywandev](https://github.com/paweldywandev)
- Website: [https://paweldywandev.com/](https://paweldywandev.com/)

## Acknowledgments

- Built with [ASP.NET Core](https://dotnet.microsoft.com/apps/aspnet)
- Frontend powered by [React](https://react.dev/) and [Vite](https://vitejs.dev/)
- UI components from [Bootstrap](https://getbootstrap.com/) and [Reactstrap](https://reactstrap.github.io/)
- Database access with [Entity Framework Core](https://docs.microsoft.com/en-us/ef/core/)
- Object mapping with [AutoMapper](https://automapper.org/)
- API documentation with [Swagger/OpenAPI](https://swagger.io/)

---

For questions, issues, or feature requests, please visit the [GitHub repository](https://github.com/paweldywandev/ImageGallery) and open an issue.

