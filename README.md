# Demo de ModelOps: Modelo de Cobranzas

**Paso 1: Clona el repositorio desde la Terminal de Jupyterlab**

File -> New Launcher -> Terminal (Icono $ grande sobre un recuadro negro)

Una vez en la raíz, pegar el comando: 

```
git clone https://github.com/lcajachahua/modelo-cobranza-servicios.git
```



**Paso 2: Carga los datos del Proyecto**

Dentro de la carpeta 'Data' se encuentra el notebook 'Data_Loading.ipynb' que nos ayuda a cargar los datos para entrenar y desplegar el modelo.Abra el notebook y ejecute el primer bloque de códigos.




**Paso 3: Creando el Proyecto en ModelOps**

Ingrese a ModelOps indicando en el usuario 'demo_user' y como password, el que eligió para el environment

Ingrese a la pestaña 'Projects' y haga clic en el botón "Create Project". Luego ingrese los siguientes datos:

    Nombre: Modelo de Cobranzas
    Detalles: Modelo de Predicción de Incumplimiento de Pago
    Repo: https://github.com/lcajachahua/modelo-cobranza-servicios.git
    Grupo: DEMO
    Credenciales: No Credentials
    Branch: main 

Cuando haya terminado, elija la opción "Save & Continue"

En la sección de “Service Connections” sólo avance con “NEXT”.

En el siguiente paso, elija “ADD PERSONAL CONNECTION” e ingrese los siguientes datos

    Nombre: conv
    Descripción: Conexion a Vantage
    Host Name: <Indique la del Servidor>
    Database Name: demo_user
    VAL Database Name: VAL
    BYOM Database Name: MLDB
    Login Mechanism: TDNEGO
    Username: demo_user
    Password: <Password de la BD>



**Paso 4: Creación de Datasets en ModelOps**    

Ingrese a la pestaña Datasets

Clic en el botón “CREATE DATASET TEMPLATE” y empiece con la configuración:

    Nombre: Dataset Cobranzas
    Descripción: Datasets para el Modelo
    Feature Catalog: Vantage
    Tags: <Los que considere>

    Data Statistics
    Database: demo_user
    Table: matriz_modelo_metadata


“NEXT” y luego en “Features”, ingresar el siguiente Query. Luego dar clic en “RUN” para ver todas las variables del dataset.

    select party_id,sex,pay_1,age,income,cv_lpay_tot,cv_lbill_tot,cant_pay_may0,bill_amt1,log_bill_amt1,avg_lpay_tot,std_pay_tot from matriz_modelo



“NEXT” y luego en “Entity & Target”, ingresar el siguiente Query y dar clic en el botón RUN

    select party_id,tar from matriz_modelo

colocar el check a la variable target (tar)
   

“NEXT” y luego en “Predictions”. Finalmente, debe indicar la tabla de Predicciones:

    Database: demo_user
    Table: matriz_modelo_predictions
    Entity selection: select * from matriz_score


Luego de ingresar el query, presione “PREVIEW DATA” y con eso ya puede finalizar, presionando “CREATE”.


Creando el Dataset tipo Train en ModelOps (Ingrese al Template creado en el paso anterior y utilice el botón “CREATE DATASET”)

Ingresar los siguientes datos

    Name: cobranzas_train
    Description: Dataset de Entrenamiento para el Modelo de Cobranzas

Finalice la creación presionando el boton “CREATE”


Creando el Dataset de Validacion en ModelOps (Utilice el botón “CREATE DATASET”)

    Name: cobranzas_val
    Description: Dataset de Validación para el Modelo de Cobranzas

Presione “NEXT” y pegue el siguiente Query:

    select party_id, tar from matriz_modelo where party_id mod 2=1


Finalice la creación presionando el boton “CREATE”. Al final, podemos ver ambos Datasets creados.


**Paso 5: El modelo está listo para ser utilizado desde el entorno del proyecto**

Regrese a la vista del Proyecto e ingrese para poder iniciar con la ejecución del Modelo

