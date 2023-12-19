# Deployment Documentation for ReactJS and Node.js Full Stack Project on IIS



## Backend (Node.js) Deployment:

#### 1. Install Node.js and Dependencies
After setting up the server folder:

```bash
# Navigate to the server folder
cd server

# Install dependencies
npm install
```

#### 2. Install Node.js and DependencieRun the Node.js Application:


```bash
# Run the Node.js application
node src/index.js
```
#### 3. Deploy Backend on IIS:
##### Steps:
### 1. Create Site and Application Pool:

##### Open IIS Manager.
##### In the Connections pane, right-click on "Sites" and select "Add Website."
##### Enter the site name, set the physical path to the server folder, and configure the port.
##### Create an Application Pool for the site.

### 2. Add web.config for Node.js:
##### Create a `web.config` file in the server folder and add the following configuration:

```bash
<configuration>
    <system.webServer>
        <handlers>
            <add name="iisnode" path="src/index.js" verb="*" modules="iisnode" />
        </handlers>
        <rewrite>
            <rules>
                <rule name="nodejs">
                    <match url="(.*)" />
                    <conditions>
                        <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" />
                    </conditions>
                    <action type="Rewrite" url="src/index.js" />
                </rule>
            </rules>
        </rewrite>
        <security>
            <requestFiltering>
                <hiddenSegments>
                    <add segment="node_modules" />
                    <add segment="iisnode" />
                </hiddenSegments>
            </requestFiltering>
        </security>
    </system.webServer>
</configuration>
```
##### If the `index.js` file is not in the src folder, adjust the paths accordingly.

### 3. Configure Permissions:
##### Right-click on the server folder.
##### Go to "Properties" -> "Security" -> "Edit" to add the necessary permissions for the IIS_IUSRS group.

### 4. Verify Deployment:
##### Open a web browser and navigate to the configured URL. If everything is set up correctly, the Node.js application should be accessible.



# Frontend (ReactJS) Deployment:

### 1. Build ReactJS Application:
* ##### After the development of the ReactJS project, build the application using the following npm commands in the client folder:

```bash
# Navigate to the client folder
cd client

# Install dependencies
npm install

# Build the application
npm run build
```

### 2. Add web.config for ReactJS:
##### Create a `web.config` file in the public folder and add the following configuration:

```bash
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <rule name="React Routes" stopProcessing="true">
          <match url=".*" />
          <conditions logicalGrouping="MatchAll">
            <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" />
            <add input="{REQUEST_FILENAME}" matchType="IsDirectory" negate="true" />
          </conditions>
          <action type="Rewrite" url="/" />
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```

### 3. Deploy Frontend on IIS:
#### Prerequisites:

* #### Node.js installed on the local machine.
* #### IIS (Internet Information Services) installed on the local machine.
* #### URL Rewrite Module installed in IIS.

### Steps:

#### 1. Install IIS and URL Rewrite Module:
* ##### Make sure IIS is installed on your local machine. You can install the URL Rewrite Module using the IIS Manager or download it from the Microsoft website.

#### 2. Create a Site and Application Pool:

* ##### Open IIS Manager.
* ##### In the Connections pane, right-click on "Sites" and select "Add Website."
* ##### Enter the site name, set the physical path to the build folder of the ReactJS application, and configure the port.
* ##### Create an Application Pool for the site.

### 3. Configure Permissions:
* ##### Right-click on the build folder of the ReactJS application.
* ##### Go to "Properties" -> "Security" -> "Edit" to add the necessary permissions for the IIS_IUSRS group.

### 4. Configure Site and Application:
* ##### In IIS Manager, select the created site.
* ##### Double-click on "Authentication" and enable "Anonymous Authentication."
* ##### Double-click on "Handler Mappings" and ensure that "iisnode" is listed.

### 5.Verify Deployment:
* ##### Open a web browser and navigate to the configured URL. If everything is set up correctly, the ReactJS application should be accessible.

## Note:
* #### Ensure that the Node.js application is deployed before the ReactJS application, as the Node.js URL path will be used for API integration in the frontend.
* #### Modify configurations and paths according to your project structure.

