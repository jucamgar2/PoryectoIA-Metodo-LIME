# PoryectoIA
Utilizaremos este repositorio para gestionar todos los recursos necesarios para el proyecto de IA

## Cosas a entregar:
* Memoria, con las explicación de los métodos aplicados, el estado del arte, los resultados y conclusiones.
* Código.

# Trabajo a realizar:
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
Es un método XAI basado en un modelo subrogado. Estos modelos subrogados deben ser modelos de caja blanca faciles de interpretar. En LIME el modelo se entrena para aproximar una predicción individual y las predicciones de su vecindad, obtenidas al perturbar la muestra individual estudiada. Dada una muestra, generaremos una nueva muestra cuyos valores de sus atributos sean 1 o 0. (1=perturbado,0=no perturbado).

Las explicaciones obtenidas con LIME se pueden expresar como:

![image](https://user-images.githubusercontent.com/72499336/233856273-3ae0bb0a-52bc-4bed-bbe3-ce6cc02ee30d.png)

Donde g es un modelo subrogado de la clase G perteneciente al grupo de modelos interpretables. El componenete omega(g) se usa como regularización para mantener la complejidad de g baja, ya que la alta complejidad se opone a la interpretabilidad. El modelo que se está explicando se denota como f y L determina el rendimineto de g ajustando la localidad definida por pi como una función de medición de proximidad como pix=pi(x,.). Cada muestra de entrenamiento se pondera con la distancia entre la muestra perturbada y la muestra original. La distancia que se suele usar es la distancia coseno.

Como modelo subrogado en este trabajo, se usará la regresión ridge. Una vez entrenado, la explicación será los pesos de la regresión. Asi consideramos dichos pesos como la importancia que cada atributo tiene para la predicción estudiada.

Pseudocódigo LIME:

![image](https://user-images.githubusercontent.com/72499336/233856520-062bd305-96ab-4fc9-91ed-306ef5bb4339.png)

## SHAP
Por el momento no resumo nada porque lo más probable es que tiremos por LIME ya que parece que se entiende mejor. Solo si no somos capaces de salir adelante com LIME, utilizaremos SHAP.

## Métricas
* Identidad: El principio de identidad establece que objetos idénticos debe recibir explicaciones idénticas. Esto estima el nivel de no determinismo intrínseco en el método.

![image](https://user-images.githubusercontent.com/72499336/233856717-2565ecd1-4959-485d-8f62-5da5a9492119.png)

donde x son muestras, d es una función de distancia y e son vectores de explicación.

* Separabilidad: Objetos no idénticos no pueden tener explicaciones idénticas

![image](https://user-images.githubusercontent.com/72499336/233856790-b11abf8c-e827-4ece-b9cf-d0ddbf7530bd.png)

Si una característica no es necesaria para la predicción, dos muestras que solo se diferencian en esa característica, tendrán la misma predicción. En este caso, se podría proporcionar la misma explicación, aunque las muestras sean diferentes. Esta métrica supone que todas las características tendrán un nivel mínimo de importancia en las predicciones.

* Estabilidad: Objetos similares deben tener explicaciones similares. un m´etodo de explicaci´on solo debe devolver explicaciones similares para objetos ligeramente diferentes.  correlación de Spearman ρ:
 
![image](https://user-images.githubusercontent.com/72499336/233857044-a7fd0b11-6f83-4204-ac74-4c5d8f68b0a2.png)

* Selectividad: La eliminación de variables relevantes debe afectar negativamente a la predicción. Para calcular la selectividad, las características se ordenan de más a menos relevante. Una por una se eliminan, dejandolas a 0, y se obtienen los errores residuales de obtener el área bajo la curva.

* Coherencia: Se calcula la diferencia entre el error de predicción sobre la señal original y el error de predicción de una nueva señal donde se eliminanan las características no importantes:

![image](https://user-images.githubusercontent.com/72499336/233857142-07db69d8-2186-4c1e-925b-065a32170dba.png)

* Completitud: Evalúa el procentaje de error de explicación con respecto al error de predicción:

![image](https://user-images.githubusercontent.com/72499336/233857183-3777aca5-f6d4-4ad1-a45e-b64a3a4f86ac.png)

* Congruencia: La desviación estándar de la coherencia proporciona el proxy de congruencia. Esta métrica ayuda a capturar la variabilidad de la coherencia:

![image](https://user-images.githubusercontent.com/72499336/233857229-cf3401cd-f8e7-482e-81b7-321b7f42d604.png)

![image](https://user-images.githubusercontent.com/72499336/233857242-de31dc20-14db-47f5-a5c0-6407b3df3245.png)


