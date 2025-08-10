# **Multicriteria Evaluation of Opportunistic Routing in Medical Emergencies (ONE Simulator)**

![Java](https://img.shields.io/badge/Java-8%2B-blue?logo=java&logoColor=white)
![ONE Simulator](https://img.shields.io/badge/Simulator-ONE-brightgreen?logo=network)
![Status](https://img.shields.io/badge/Status-Completed-success)

## **Overview**
This project evaluates **Epidemic** and **Spray-and-Wait** routing protocols in a **medical emergency** scenario using the **Opportunistic Network Environment (ONE)** simulator. The goal is to understand trade-offs across delivery, delay, overhead, drops, and hop count when connectivity is intermittent (DTN/Opportunistic networks).

---

## **Node Groups (35 Total)**

1. **G1 – Patient (1 node):** stationary at (1000, 1000)  
2. **G2 – Health workers (10):** shortest-path map-based movement; speed 1–3.5 m/s  
3. **G3 – Travellers (8):** random waypoint; speed 0.5–3 m/s  
4. **G4 – Vehicles (15):** map-based movement; speed 1–10 m/s; uses 4 map layers  
   - Roads  
   - Main roads  
   - Pedestrian paths  
5. **G5 – Health centre (1):** stationary at (2000, 2600)  

---

## **Protocols Compared**
- **Epidemic:** flooding/replication for high delivery probability; can cause buffer pressure and high overhead; mitigated by TTL and peer caches  
- **Spray-and-Wait:** controlled replication (spray then direct wait to destination); lower overhead; may trade delivery for delay in sparse networks  

---

## **Metrics Evaluated**
- **Created**
- **Delivered**
- **Dropped**
- **Latency_avg**
- **Overhead_ratio**
- **Hopcount_avg**

---

## **Key Findings**
- **Epidemic**:
  - Best delivery when buffers are large (e.g., 814 delivered @ 500 MB)  
  - Suffers **very high overhead** and drops when buffers are small  
- **Spray-and-Wait**:
  - Keeps **overhead low** and drops fewer messages at small/medium buffers  
  - Delivery plateaus as buffers grow  
- **Recommendation**: Spray-and-Wait is better in **resource-limited emergencies**; Epidemic works if overhead is acceptable and buffers are large  

---

## **Representative Results**
| Protocol        | Buffer | Delivered | Dropped  | Latency_avg | Overhead_ratio | Hopcount_avg |
|-----------------|-------:|----------:|---------:|------------:|---------------:|-------------:|
| Epidemic        | 5 MB   | 46        | 14,040   | 4444.15     | 286.72         | 7.52         |
| Epidemic        | 50 MB  | 232       | 109,415  | 5929.66     | 476.30         | 10.09        |
| Epidemic        | 500 MB | 814       | 20,965   | 4353.77     | 38.91          | 4.33         |
| Spray-and-Wait  | 5 MB   | 68        | 5,265    | 4921.77     | 64.94          | 2.60         |
| Spray-and-Wait  | 50 MB  | 156       | 4,915    | 6396.17     | 31.99          | 2.54         |
| Spray-and-Wait  | 500 MB | 156       | 3,678    | 6396.17     | 31.99          | 2.54         |

---

## **How to Reproduce**

### **Prerequisites**
- **Java 8+**  
- **ONE Simulator** (place `one.jar` at repo root or adjust paths)  
- **Config file**: `config/medical_emergency_settings.txt`  
- **Map files** (in `data/` directory):  
  - `roads.wkt`  
  - `main_roads.wkt`  
  - `pedestrian_paths.wkt`  
  - `shops.wkt`  

### **Run**
```bash
# From the ONE simulator directory
java -jar one.jar config/medical_emergency_settings.txt
