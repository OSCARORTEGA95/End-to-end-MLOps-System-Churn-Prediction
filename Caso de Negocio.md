### **Caso de Negocio: Optimización de la Retención en SaaS-Vibe, una Plataforma B2B de Gestión de Operaciones**







**1. El Contexto de la Empresa**



SaaS-Vibe es una plataforma de software en la nube bajo modelo de suscripción mensual que ayuda a medianas y grandes empresas a coordinar sus flujos de trabajo, logística y reportes operativos. El costo promedio de la suscripción por cliente corporativo es de $500 USD al mes.



**2. La Problemática (El Dolor del Negocio)**



En los últimos tres trimestres, el equipo de Operaciones y Finanzas detectó un incremento alarmante en la tasa de abandono mensual (Churn Rate). Actualmente, el Churn se sitúa en el 4.5% mensual, lo cual está muy por encima del objetivo de la industria para el sector B2B (que idealmente debe ser menor al 2%).



El impacto financiero directo se calcula bajo la siguiente premisa:



•	Base de clientes actuales: 2,000 empresas.

•	Pérdida mensual actual (4.5%): 90 clientes perdidos al mes.

•	Impacto financiero inmediato: $45,000 USD de MRR (Ingreso Mensual Recurrente) perdidos cada mes.



Además, el Costo de Adquisición de Clientes (CAC) es de $1,500 USD, lo que significa que recuperar el ritmo de ingresos quemando presupuesto en marketing no es sostenible. Es 5 veces más barato retener a un cliente actual que adquirir uno nuevo.



**3. Las Fuentes de Datos Disponibles**



Para resolver este problema, el equipo de datos tiene acceso a tres fuentes principales en el Data Warehouse corporativo:



•	datos\_clientes (Demográficos y Contrato): Antigüedad del cliente, sector industrial, número de licencias contratadas, tipo de soporte (básico/premium) y método de pago.

•	uso\_plataforma (Comportamiento digital): Frecuencia de inicio de sesión, tiempo de uso por sesión, número de reportes generados, y clics en funciones críticas del software en los últimos 30 días.

•	soporte\_tickets (Interacción y Fricción): Número de quejas radicadas, tiempo de resolución de fallas técnicas y calificaciones de satisfacción del servicio al cliente (CSAT).



**4. El Objetivo del Proyecto de Datos**



El objetivo es diseñar e implementar un sistema de alerta temprana que logre dos cosas:



1\.	Predecir con una ventana de 30 días de anticipación qué clientes tienen una probabilidad superior al 70% de abandonar la plataforma.

2\.	Entregar al equipo de Customer Success (Éxito del Cliente) las razones principales de la predicción para que puedan ejecutar una estrategia de retención personalizada (ej. llamadas de soporte prioritario, descuentos temporales o capacitación gratuita).



**Restricciones Financieras de la Solución (Métricas de Negocio)**



El equipo de Customer Success tiene un presupuesto limitado para ofrecer incentivos de retención. Solo pueden intervenir al 15% de la base total de clientes al mes. Por lo tanto, el modelo de Machine Learning no solo debe ser preciso, sino que debe maximizar el Precision y el Recall en el percentil más alto de riesgo para evitar falsos positivos que desperdicien presupuesto.









