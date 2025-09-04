# MLOps Experiments with MLFlow

Complete demonstration of ML experiment tracking using MLFlow with both local and remote server architectures.

## Project Workflow

### 1. Environment Setup
```bash
pip install mlflow pandas scikit-learn  # Install dependencies
```

### 2. Local MLFlow Server
```bash
mlflow ui --host 0.0.0.0 --port 5000  # Start local MLFlow UI
```

### 3. Remote Tracking (DagsHub)
```bash
export MLFLOW_TRACKING_URI="https://dagshub.com/[username]/[repo].mlflow"  # Set remote URI
export MLFLOW_TRACKING_USERNAME="your-username"  # Set credentials  
export MLFLOW_TRACKING_PASSWORD="your-token"     # Set access token
```

### 4. Experiment Tracking
```python
import mlflow
import mlflow.sklearn

mlflow.set_experiment("experiment-name")  # Create/set experiment
with mlflow.start_run():                  # Start tracking run
    mlflow.log_param("param", value)      # Log parameters
    mlflow.log_metric("accuracy", score)  # Log metrics
    mlflow.sklearn.log_model(model, "model")  # Log model
```

### 5. Model Registry
```python
mlflow.register_model("runs:/run-id/model", "ModelName")  # Register model
```

## Key Concepts

### Local vs Remote Server Architecture

| Aspect | Local Server | Remote Server |
|--------|--------------|---------------|
| **Location** | Runs on your machine (localhost:5000) | Hosted on AWS/DagsHub/Cloud |
| **Access** | Only you can see experiments | Team members can collaborate |
| **Internet** | Works offline | Requires internet connection |
| **Use Cases** | • Individual development<br>• Testing<br>• Quick prototyping | • Team collaboration<br>• Production deployments<br>• Centralized tracking |
| **Control** | Full control over everything | Shared/managed environment |

### MLFlow Server Architecture Comparison

| Feature | AWS Architecture | DagsHub Architecture |
|---------|------------------|---------------------|
| **Setup Complexity** | High (EC2 + S3 + RDS setup) | Low (just sign up) |
| **Management** | You manage servers/databases | Fully managed service |
| **Scaling** | Manual scaling required | Automatic scaling |
| **Cost** | Pay for infrastructure | Free tier available |
| **Control** | Complete control | Limited customization |
| **Integration** | Custom integrations | Built-in Git integration |
| **Best For** | Large teams, enterprise | Small teams, quick start |

### Model Registry

**Core Concept:** Centralized warehouse for trained ML models with versioning and lifecycle management

**Key Features:**
- Automatic version numbering (v1, v2, v3...)
- Stage management (None → Staging → Production)
- Model comparison across versions
- Easy rollback capabilities
- Team access to same models

**Typical Workflow:**
1. Train 10 models locally
2. Register best 3 models in registry
3. Move selected model to "Staging" 
4. Test model in staging environment
5. Promote to "Production" for live use
6. Monitor and rollback if needed

**Example Stages:**
- **None:** Just registered, not deployed
- **Staging:** Being tested before production
- **Production:** Live model serving predictions
- **Archived:** Old versions kept for reference

## Commands Reference

```bash
mlflow experiments list                    # List all experiments
mlflow runs list --experiment-id 1        # List runs in experiment
mlflow models serve -m models:/ModelName/1 -p 1234  # Serve model locally
mlflow ui --backend-store-uri sqlite:///mlruns.db   # Use SQLite backend
```
