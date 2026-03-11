**1. ¿Qué problema humano resuelve?** La ansiedad y el rechazo del operador ante la caja negra de la IA. Un operador de 25 años de experiencia no ejecutará un setpoint si no entiende _por qué_ y _qué pasará_.

**2. ¿Cuál es su única razón de existir?** Traducir tensores matemáticos y setpoints propuestos en proyecciones físicas y económicas comprensibles para un humano.

**3. ¿Qué pasa cuando esto cambie?** Cambiaremos las métricas (añadir proyección de emisiones de CO2 además de consumo energético) o la forma de explicar el peso de las variables (usando valores SHAP, por ejemplo). El `Runtime` (que solo calcula) y el UI (que solo renderiza) no sufren impacto.

**4. ¿Puedo eliminarlo sin romper lo no relacionado?** Sí. El `Runtime` seguirá calculando setpoints y el `Gateway` podrá escribirlos (si está en Closed-Loop). Se pierde la confianza humana y la explicabilidad, pero el lazo de control matemático sigue intacto.

**Estructura Interna:**

Plaintext

```
src/
└── operator-observability/
    ├── domain/
    │   ├── economics/         # Lógica de cálculo: $\Delta Energía \times Precio$
    │   ├── physics-projection/# Extrapolación a corto plazo del impacto del setpoint
    │   └── shap-analyzer/     # Explicabilidad matemática del modelo ONNX
    ├── application/
    │   └── narrative-engine/  # Construye el "Por qué" basado en los dominios
    └── api/
        └── JustificationAPI   # Contrato consumido por el Dashboard
```