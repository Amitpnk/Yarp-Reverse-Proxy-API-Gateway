# **YARP Reverse Proxy Demo**

This repository demonstrates how to build a **reverse proxy** using **YARP (Yet Another Reverse Proxy)** in **.NET 8**, along with two backend microservices.
It is a great starting point to create a **simple API Gateway** or **service routing solution** in microservices architecture.

## **Projects Overview**

The solution contains **three projects**:

1. **YarpReverseProxyDemo** – The reverse proxy project using YARP.
2. **Service1** – A dummy backend API returning weather data.
3. **Service2** – Another dummy backend API returning weather data.

```
src/
│── YarpReverseProxyDemo/         # Reverse proxy project
│   ├── Program.cs
│   ├── appsettings.json
│   └── ...
│
│── Service1/                     # Backend service 1
│   ├── Controllers/WeatherForecast1Controller.cs
│   ├── WeatherForecast.cs
│   └── ...
│
│── Service2/                     # Backend service 2
│   ├── Controllers/WeatherForecast2Controller.cs
│   ├── WeatherForecast.cs
│   └── ...
│
└── README.md
```

## **How It Works**

* **YARP** acts as a **reverse proxy** and routes incoming requests to the backend services based on **configured routes and clusters**.
* Routes:

  * `/service1/*` → **Service1**
  * `/service2/*` → **Service2**

**appsettings.json example:**

```json
{
  "ReverseProxy": {
    "Routes": {
      "service1-route": {
        "ClusterId": "service1-cluster",
        "Match": {
          "Path": "api/WeatherForecast1/{**catch-all}"
        }
      },
      "service2-route": {
        "ClusterId": "service2-cluster",
        "Match": {
          "Path": "api/WeatherForecast2/{**catch-all}"
        }
      }
    },
    "Clusters": {
      "service1-cluster": {
        "Destinations": {
          "service1-destination": {
            "Address": "https://localhost:5051/"
          }
        }
      },
      "service2-cluster": {
        "Destinations": {
          "service2-destination": {
            "Address": "https://localhost:5052/"
          }
        }
      }
    }
  }
}

```

## **Setup & Run**

### **1. Clone the repository**

```bash
git clone https://github.com/Amitpnk/YarpReverseProxyDemo.git
cd YarpReverseProxyDemo
```

### **2. Restore NuGet packages**

```bash
dotnet restore
```

### **3. Run Backend Services**

Run **Service1** and **Service2** projects on different ports:

```bash
dotnet run --project Service1
dotnet run --project Service2
```

* **Service1** default: [https://localhost:5051](https://localhost:5051)
* **Service2** default: [https://localhost:5052](https://localhost:5052)

### **4. Run the YARP Reverse Proxy**

```bash
dotnet run --project YarpReverseProxyDemo
```

* Proxy will run on: **[https://localhost:5050](https://localhost:5050)**

---

## **5. Test the Proxy**

Use **curl**, **Postman**, or a browser:

```bash
curl --location 'https://localhost:5051/api/weatherforecast1'
curl --location 'https://localhost:5052/api/weatherforecast1'

curl --location 'https://localhost:5050/api/weatherforecast1'
curl --location 'https://localhost:5050/api/weatherforecast2'
```

You should see JSON responses from the respective backend services routed via the proxy.


## **Features**

* Simple **YARP reverse proxy** in .NET
* **Route and cluster configuration** via `appsettings.json`
* **Multiple backend services** demonstration
* Ready to extend for **authentication, logging, load balancing, and caching**

## **Next Steps**

* Add **JWT authentication** to protect services.
* Implement **rate limiting and caching**.
* Deploy the solution using **Docker & Kubernetes**.

## **References**

* [YARP Official Documentation](https://microsoft.github.io/reverse-proxy/)
* [.NET Documentation](https://learn.microsoft.com/en-us/dotnet/core/)


If you find this project helpful, ⭐ the repository and follow for more **.NET microservices tutorials**. 