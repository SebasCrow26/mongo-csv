# MongoDB - Reflexión y Código

La interaccion con MongoDB fue bastante sencilla, con la complementacion que vimos la clase anterior y junto al paso a paso que la profe nos dejo tanto con las imagenes como de manera presencial del como crear las conecciones en mongo y sus respectivos codigos en python, facilitaron bastante los pasos a seguir.

El codigo fue:

```python
# Instalar PyMongo
!pip install --upgrade pymongo

# Importar librería
from pymongo import MongoClient

# Conexión a tu base de datos Atlas
import certifi
from pymongo import MongoClient

cliente = MongoClient(
    "mongodb+srv://sebastianramos26122005_db_user:SebDRC2612.@cluster0.polihpg.mongodb.net/?appName=Cluster0",
    tls=True,
    tlsCAFile=certifi.where()
)

try:
    cliente.admin.command('ping')
    print("Conexion exitosa a MongoDB Atlas!")
except Exception as e:
    print("Error de conexion:", e)

# Seleccion de base de datos y coleccion
db = cliente["BD_CursoMongo"]
estudiantes = db["Estudiantes"]

# Funciones CRUD basicas

def insertar_estudiante(nombre, edad, carrera):
    estudiante = {"nombre": nombre, "edad": edad, "carrera": carrera}
    estudiantes.insert_one(estudiante)
    print(f"Estudiante {nombre} insertado.")

def listar_estudiantes():
    lista = list(estudiantes.find())
    if not lista:
        print("No hay estudiantes registrados.")
    else:
        print(f"\n{'NOMBRE':<25} {'EDAD':<6} {'CARRERA'}")
        print("-" * 50)
        for est in lista:
            print(f"{est.get('nombre',''):<25} {est.get('edad',''):<6} {est.get('carrera','')}")
        print(f"\nTotal: {len(lista)} estudiantes")

def actualizar_edad(nombre, nueva_edad):
    resultado = estudiantes.update_one({"nombre": nombre}, {"$set": {"edad": nueva_edad}})
    if resultado.matched_count > 0:
        print(f"Edad de {nombre} actualizada a {nueva_edad}.")
    else:
        print(f"No se encontro {nombre}.")

def eliminar_estudiante(nombre):
    resultado = estudiantes.delete_one({"nombre": nombre})
    if resultado.deleted_count > 0:
        print(f"Estudiante {nombre} eliminado.")
    else:
        print(f"No se encontro {nombre}.")

# Menu interactivo
def menu():
    while True:
        print("\n--- MENU DE ESTUDIANTES ---")
        print("1. Insertar estudiante")
        print("2. Listar estudiantes")
        print("3. Actualizar edad de un estudiante")
        print("4. Eliminar estudiante")
        print("5. Salir")

        opcion = input("Selecciona una opcion (1-5): ").strip()

        if opcion == "1":
            nombre = input("Nombre: ").strip()
            edad = int(input("Edad: ").strip())
            carrera = input("Carrera: ").strip()
            insertar_estudiante(nombre, edad, carrera)

        elif opcion == "2":
            listar_estudiantes()

        elif opcion == "3":
            nombre = input("Nombre del estudiante: ").strip()
            nueva_edad = int(input("Nueva edad: ").strip())
            actualizar_edad(nombre, nueva_edad)

        elif opcion == "4":
            nombre = input("Nombre del estudiante a eliminar: ").strip()
            eliminar_estudiante(nombre)

        elif opcion == "5":
            print("Saliendo...")
            break

        else:
            print("Opcion invalida. Intenta de nuevo.")

menu()
```

## Datos: BD_CursoMongo.Estudiantes

| nombre | edad | carrera |
|---|---|---|
| MIGUEL MORENO | 19 | ciencia de datos |
| Angie Trujillo | 18 | ciencia de datos |
| Miguel Camargo | 24 | ciencia de datos |
| Camilo Hernandez | 19 | ciencia de datos |
| Nicolas Cardenas | 19 | ciencia de datos |
| Jorge Zainea | 30 | ciencia de datos |
| Valeria Villamil | 25 | ciencia de datos |
| Julian Duarte | 20 | ciencia de datos |
| Aylin Gomez | 18 | ciencia de datos |
| Naomi Mosquera | 20 | ciencia de datos |
| Daniela Gonzalez | 20 | ciencia de datos |
| Kevin Luengas | 20 | ciencia de datos |
| Laura Saenz | 24 | ciencia de datos |
| Eylin Herrera | 19 | ciencia de datos |
| Valentina Bello | 19 | ciencia de datos |
| Maria Guzman | 19 | ciencia de datos |
| Joshua Trujillo | 21 | ciencia de datos |
| Santiago Sandoval | 20 | Ciencia de Datos |
| Sebastian Ramos | 20 | Ciencia de Datos |

*Actividad realizada por: Sebastian Ramos — BD CursoMongo | MongoDB con Python*
