# Low-Level Design (LLD)

## 📌 System Title
AI-Powered Restaurant Demand, Staffing, and Inventory Optimization System

---

## 🎯 1. Objective

Define detailed implementation design including:
- Database schema
- Service-level APIs
- Class structures
- Data contracts
- Internal workflows

---

## 🗃️ 2. Database Schema Design

### 2.1 orders

| Column         | Type        | Description                |
|----------------|------------|----------------------------|
| id             | UUID (PK)  | Unique order ID           |
| timestamp      | TIMESTAMP  | Order time                |
| covers         | INT        | Number of customers       |
| total_amount   | FLOAT      | Revenue                   |

---

### 2.2 order_items

| Column     | Type        | Description              |
|------------|------------|--------------------------|
| id         | UUID (PK)  | Unique ID                |
| order_id   | UUID (FK)  | Linked order             |
| item_id    | UUID (FK)  | Menu item                |
| quantity   | INT        | Quantity ordered         |

---

### 2.3 menu_items

| Column     | Type        | Description              |
|------------|------------|--------------------------|
| id         | UUID (PK)  | Item ID                  |
| name       | VARCHAR    | Item name                |
| price      | FLOAT      | Price                    |

---

### 2.4 ingredients

| Column             | Type     | Description              |
|--------------------|----------|--------------------------|
| id                 | UUID     | Ingredient ID            |
| name               | VARCHAR  | Ingredient name          |
| shelf_life_days    | INT      | Expiry duration          |
| lead_time_days     | INT      | Supplier delay           |
| unit_cost          | FLOAT    | Cost per unit            |

---

### 2.5 recipe_map

| Column        | Type   | Description              |
|---------------|--------|--------------------------|
| item_id       | UUID   | Menu item                |
| ingredient_id | UUID   | Ingredient               |
| quantity      | FLOAT  | Quantity per dish        |

---

### 2.6 staff

| Column            | Type     | Description              |
|-------------------|----------|--------------------------|
| id                | UUID     | Staff ID                 |
| role              | VARCHAR  | Role (chef/waiter)       |
| efficiency_factor | FLOAT    | Covers per hour          |

---

### 2.7 forecasts

| Column           | Type      | Description              |
|------------------|-----------|--------------------------|
| id               | UUID      | Forecast ID              |
| date             | DATE      | Forecast date            |
| hour             | INT       | Hour of day              |
| predicted_covers | INT       | Predicted customers      |

---

### 2.8 actuals

| Column        | Type      | Description              |
|---------------|-----------|--------------------------|
| id            | UUID      | Actual record ID         |
| date          | DATE      | Date                     |
| hour          | INT       | Hour                     |
| actual_covers | INT       | Actual customers         |

---

### 2.9 feedback

| Column    | Type      | Description              |
|-----------|-----------|--------------------------|
| id        | UUID      | Feedback ID              |
| date      | DATE      | Date                     |
| predicted | INT       | Predicted value          |
| actual    | INT       | Actual value             |
| reason    | VARCHAR   | Reason (rain/event)      |
| notes     | TEXT      | Manager comments         |

---

## 🧠 3. Core Service Design

---

### 3.1 Forecasting Service

Class: ForecastService

    class ForecastService:

        def predict_hourly_demand(self, date: datetime) -> List[Forecast]:
            pass

        def load_model(self):
            pass

        def preprocess_features(self, data):
            pass

---

### 3.2 Staff Optimization Service

Class: StaffPlanner

    class StaffPlanner:

        def calculate_staff(self, forecasts: List[Forecast]) -> Dict:
            pass

        def apply_constraints(self, staff_plan):
            pass

---

### 3.3 Inventory Planning Service

Class: InventoryPlanner

    class InventoryPlanner:

        def estimate_ingredients(self, forecasts: List[Forecast]) -> Dict:
            pass

        def apply_safety_stock(self, plan):
            pass

---

### 3.4 Feedback Service

Class: FeedbackService

    class FeedbackService:

        def store_feedback(self, feedback_data):
            pass

        def calculate_error(self, predicted, actual):
            return actual - predicted

---

### 3.5 Learning Engine

Class: LearningEngine

    class LearningEngine:

        def retrain_model(self):
            pass

        def update_weights(self, error_data):
            pass

        def train_residual_model(self):
            pass

---

## 🔌 4. API Design

### 4.1 GET /forecast

Response:

    {
      "date": "2026-05-10",
      "hourly_forecast": [
        {"hour": 18, "covers": 20},
        {"hour": 19, "covers": 45}
      ]
    }

---

### 4.2 GET /staff-plan

Response:

    {
      "date": "2026-05-10",
      "staff": {
        "waiter": 5,
        "chef": 3
      }
    }

---

### 4.3 GET /inventory-plan

Response:

    {
      "date": "2026-05-10",
      "ingredients": {
        "flour": "8kg",
        "tomato": "5kg"
      }
    }

---

### 4.4 POST /feedback

Request:

    {
      "date": "2026-05-10",
      "predicted": 120,
      "actual": 85,
      "reason": "rain",
      "notes": "heavy rain reduced walk-ins"
    }

---

## 🔄 5. Internal Workflows

### 5.1 Forecast Flow
- Fetch historical data
- Add external features
- Generate features
- Run model
- Store predictions

---

### 5.2 Staff Planning Flow
- Read forecast output
- Apply staff efficiency rules
- Run constraint solver
- Return optimized staffing

---

### 5.3 Inventory Planning Flow
- Convert covers → dish demand
- Map dishes → ingredients
- Apply shelf life constraints
- Add safety stock

---

### 5.4 Feedback Loop Flow
- Capture actual data
- Compute prediction error
- Store feedback
- Trigger model update

---

## ⚙️ 6. Background Jobs

| Job Name           | Frequency | Description               |
|--------------------|----------|---------------------------|
| Forecast Batch Job | Daily    | Generate next-day forecast|
| Model Retraining   | Weekly   | Retrain ML models         |
| Data Cleanup       | Daily    | Remove stale data         |

---

## 🧪 7. Validation & Error Handling

- Input validation via Pydantic
- Graceful fallback if model fails
- Default rule-based outputs

---

## 🔐 8. Security (Implementation Level)

- JWT authentication middleware
- Role-based endpoint access
- Input sanitization

---

## 📊 9. Logging & Monitoring

- Structured logging (JSON)
- Error tracking
- Model accuracy tracking

---

## 🧠 10. Key Design Patterns Used

- Repository Pattern → DB access abstraction
- Service Layer Pattern → Business logic separation
- Factory Pattern → Model selection
- Strategy Pattern → Forecasting algorithms
- Observer Pattern → Feedback triggering retraining

---

## 🚀 11. Deployment Structure

/app  
  /api  
  /services  
  /models  
  /schemas  
  /db  
  /jobs  

---

## 📌 12. Summary

This LLD defines the concrete implementation blueprint for building a modular, scalable, and feedback-driven system. It ensures separation of concerns, maintainability, and readiness for production deployment.
