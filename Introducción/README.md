# Objetivo del repositorio

Este repositorio detalla el proceso necesario como para obtener una instancia funcional de CALDERA en un entorno de laboratorio

# Introducción

## Que es Mitre Caldera?

MITRE CALDERA es una plataforma open source de Breach and Attack Simulation (BAS) desarrollada por MITRE Corporation. Permite emular el comportamiento de adversarios reales sobre infraestructura propia, de forma controlada y repetible, con todo mapeado al framework MITRE ATT&CK (CALDERA mapea cada tecnica ejecutada a su tactica en MITRE ATT&CK)

## Componentes principales

* Servidor CALDERA	Nucleo de la plataforma. Interfaz web + REST API. Corre en Kali.
* Agente (Sandcat)	Proceso liviano que se despliega en el target Windows.
* Abilities	Tecnicas individuales de ataque. Equivalentes a TTPs de ATT&CK.
* Adversary Profile	Coleccion de Abilities que modela un actor de amenaza real.
* Operation	Ejecucion de un Adversary Profile contra agentes activos.
* Planner	Motor que determina el orden de ejecucion de las Abilities
