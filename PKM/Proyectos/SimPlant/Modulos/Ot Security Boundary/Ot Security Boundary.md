**1. ¿Qué problema humano resuelve?** Garantiza que nadie pueda hackear la planta desde internet y volar una refinería manipulando los modelos o los setpoints desde la red corporativa.

**2. ¿Cuál es su única razón de existir?** Implementar el aislamiento físico/lógico (Modelo Purdue) y validar criptográficamente cualquier modelo u orden que cruce desde IT (Trainer) hacia OT (Runtime).

**3. ¿Qué pasa cuando esto cambie?** Cambian las estrategias de firma de archivos, protocolos de _data diodes_ o certificaciones IEC 62443. La lógica de control del `Runtime` no se toca.

**4. ¿Puedo eliminarlo sin romper lo no relacionado?** A nivel de código, sí (podrías hacer un fetch HTTP simple y todo "funciona"). A nivel de despliegue, eliminarlo viola el Axioma Cero: deja de resolver el problema de forma segura y causa la muerte del producto (ninguna planta te comprará).

**Estructura Interna:**

Plaintext

```
src/
└── ot-security-boundary/
    ├── domain/
    │   ├── crypto/            # Verificación de firmas de modelos ONNX
    │   └── firewall-rules/    # Validadores de estado de red unidireccional
    ├── application/
    │   └── model-transfer/    # Orquesta el paso seguro del archivo ONNX de Nivel 4 a Nivel 3
    └── api/
        └── SecureDeployment   # Contrato para recibir actualizaciones de modelo
```