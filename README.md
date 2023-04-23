# PoryectoIA
Utilizaremos este repositorio para gestionar todos los recursos necesarios para el proyecto de IA

## Cosas a entregar:
* Memoria, con las explicación de los métodos aplicados, el estado del arte, los resultados y conclusiones.
* Código.

#Trabajo a realizar:
1. Implementar un método XAI
2. Implementar métricas. (Se recomienda usar numpy y spicy)
3. Seleccionar 2 conjuntos de datos.
4. Entrenar dos modelos de caja negra(random forest, xgboost, redes neuronales, ...) con cada conjunto. Modelo bién entrenado, ni sobreajustado ni infraajustado. Hay que validarlos y optimizarlo.
5. Medir las metricas sobre los modelos entrenados. debemos seleccionar al menos 256 muestras de los conjuntos de datos que no hayan sido utilizadas para entrenar los modelos.
6. Presentar los resultados y sacar conclusiones.

## Criterios de evaluación
* Corrección, claridad y comentarios en la implementación del método XAI.(1)
* Corrección, claridad y comentarios en la implementación de las métricas.(1)
* Corrección, claridad y comentarios en el entrenamiento de los modelos.(0.6)
* Medición de las métricas(0.6)
* Memoria, presentación de resultados y conclusiones(0.8)

En la defensa nos pediran:
* Aplicar el método de aplicabilidad implementado ccon una muestra y un modelo.
* Aplicar las métricas implementadas para un modelo.

## Introducción
Modelos de caja negra, modelos cuyo comportamiento es complejo y se comportan de forma opaca, además impactan en diferentes dimensiones dentro del ámbito humano. Los modelos opacos son dificiles de depurar, a diferencia de los interpretables.
La XAI aborda estos problemas proponiendo técnicas de aprendizaje automático que generan explicaciones de modelos opacos o crean modelos más transparentes. Las tecnicas agnosticas como LIME o SHAP, se pueden aplicar a cualquier modelo.
Se pueden encontrar dos tipos de indicadores para la evaluación y comparación de explicaciones: cualitativos y cuantitativos. Usaremos indicadores cuantitativos.

## Vecindad de una muestra
Los métodos que utilizaremos suponen que habrá cambios significativos en el rendimiento de un modelo si se alteran sus características relevantes. Para ello toman una muestra y van perturbando uno o varios atributos. A esta nuevas muestras perturbadas se las llama vecindad de la muestra original.
Un metodo habitual para perturbar es introducir ruido teniendo en cuenta el rango de valores del atributo.

## LIME
