# oracle-a1-capacity-retry

Reintenta cada 10 minutos crear una instancia Ampere A1 (Always Free) en Oracle Cloud
mediante el Apply de un stack de Resource Manager ya existente, hasta que haya
capacidad disponible en la región. Avisa por [ntfy.sh](https://ntfy.sh) al conseguirlo.

No contiene credenciales: usa Secrets del repositorio (`OCI_USER`, `OCI_FINGERPRINT`,
`OCI_TENANCY`, `OCI_REGION`, `OCI_KEY_CONTENT`, `OCI_STACK_ID`, `NTFY_TOPIC`).
