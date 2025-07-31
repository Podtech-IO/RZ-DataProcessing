# Azure Functions Python Setup Guide

## Prerequisites

1. Install Visual Studio Code
2. Install Python (3.8 or higher)
3. Install Azure Functions Core Tools
   ```powershell
   npm install -g azure-functions-core-tools@4 --unsafe-perm true
   ```
4. Install Azure Functions extension for VS Code
   - Search for "Azure Functions" in VS Code extensions
   - Install "Azure Functions" by Microsoft

## Step-by-Step Setup

### 1. Create Project Structure

```powershell
# Create and navigate to project directory
mkdir YourProjectName
cd YourProjectName

# Initialize Azure Functions project
func init . --python
```

### 2. Set Up Python Virtual Environment

```powershell
# Create virtual environment
python -m venv .venv

# Activate virtual environment
# On Windows:
.venv\Scripts\activate
# On macOS/Linux:
source .venv/bin/activate
```

### 3. Install Required Packages

```powershell
# Install base requirements
pip install azure-functions

# If using a database (e.g., PostgreSQL)
pip install psycopg2-binary

# For environment variables
pip install python-dotenv

# Save dependencies
pip freeze > requirements.txt
```

### 4. Create Function

```powershell
# Create a new HTTP-triggered function
func new --name YourFunctionName --template "HTTP trigger" --authlevel "anonymous"
```

### 5. Configure Local Settings

Create/update `local.settings.json`:

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "PYTHON_ISOLATE_WORKER_DEPENDENCIES": "1"
  }
}
```

### 6. Environment Variables (Optional)

Create `.env` file for local development:

```plaintext
# Database Configuration (if needed)
DATABASE_URL=your_database_connection_string

# Azure Functions Runtime
FUNCTIONS_WORKER_RUNTIME=python
AzureWebJobsStorage=UseDevelopmentStorage=true
```

### 7. VS Code Tasks Configuration

Create `.vscode/tasks.json`:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Run Function App",
      "type": "shell",
      "command": "func start",
      "problemMatcher": ["$func-python-watch"],
      "isBackground": true,
      "group": {
        "kind": "build",
        "isDefault": true
      }
    }
  ]
}
```

### 8. Start the Function App

```powershell
# Make sure you're in the project directory
func start
```

## Testing

- Your function will be available at: `http://localhost:7071/api/YourFunctionName`
- Test using tools like Postman, curl, or your browser

## Common Issues and Solutions

1. **Module Not Found Errors**

   - Ensure you're in the virtual environment
   - Verify packages are listed in requirements.txt
   - Run `pip install -r requirements.txt`

2. **Port Already in Use**

   - Stop other running functions
   - Change port in local.settings.json:
     ```json
     {
       "Values": {
         "FUNCTIONS_HTTPWORKER_PORT": "7072"
       }
     }
     ```

3. **Python Version Mismatch**
   - Check Python version: `python --version`
   - Ensure using Python 3.8 or higher
   - Create new venv with correct Python version

## Best Practices

1. Always use virtual environment
2. Keep requirements.txt updated
3. Don't commit sensitive info in local.settings.json
4. Use .gitignore (created automatically)
5. Document environment variables in README

## Deployment Preparation

1. Update requirements.txt: `pip freeze > requirements.txt`
2. Test locally before deployment
3. Configure production settings in Azure Portal
4. Remove local development values from settings

## Additional Resources

- [Azure Functions Python Developer Guide](https://docs.microsoft.com/azure/azure-functions/functions-reference-python)
- [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools)
- [Python Virtual Environments](https://docs.python.org/3/tutorial/venv.html)
