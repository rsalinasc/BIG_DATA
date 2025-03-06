# Descripción del Taller 4 (parte 1) de clase
Trabajar con servidor MongoDB

 1.Instalar librerias necesarias
 
 2.configuraciones especiales para crear un servidor MongoDB

 3.crer un BD

 4.Crear colecciones

 5.Descargar Dataset's (varios) en un .zip y cargarlos en colecciones de MongoDB

# 0.Identificación:
* Nombre: Ronald Salinas
* correo: rsalinasc@ucentral.edu.co
* asignatura: Big Data

# 1.Instalar librerias especiales

!apt update
!apt install -y python3-pip
!pip3 install pyspark   #Libreria especial

!pip3 install pymongo # Libreria especial

## 1.1 Instalar la versión mongoDB Ubuntu 22.04

!sudo apt-get install -y gnupg curl
!curl -fsSL https://pgp.mongodb.com/server-6.0.asc | sudo gpg -o /usr/share/keyrings/mongodb-server-6.0.gpg --dearmor
!echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list

!sudo apt-get update
!sudo apt-get install -y mongodb-org

# 1.Instanciar las librerias

# Habilitamos drive de google desde colab
from google.colab import drive
drive.mount('/content/drive')

import os
db_path = '/content/drive/MyDrive/UniversidadCentral/Maestría_en_Analítica_de_Datos/Bigdata/Ejercicios_de_clase/DataBase/MongoDB/'
os.makedirs(db_path, exist_ok=True)
!sudo chmod 777 $db_path  #permisos especiales de escritura sobre esa carpeta

# Modifico las variables del entorno del sistema o para que reconozca el servidor
!sudo systemctl stop mongod
!sudo mongod --dbpath $db_path --fork --logpath /var/log/mongodb/mongodb.log

# 2. Inicializar el servidor de MongoDB

from pymongo import MongoClient
client = MongoClient('localhost', 27017)

# 2.1Funciones DML(Select, insert, update, delete)

# Listar las bases de datos existentes y colecciones (tablas)
database_names = client.list_database_names()
for db_name in database_names:
  print("Base de datos: ",db_name)
  db = client[db_name]
  collection_names = db.list_collection_names()
  for collection_name in collection_names:
    print("  Colección: ",collection_name)

# 3. Crear una base de datos y colecciones

db = client['Taller4']
#-------------coleciones (tablas)---------
colle_profesores =db['profesores']

profe= colle_profesores.insert_one({"nombre":"LuisFdo","Profesion":"Ingeniero"}).inserted_id

for doc in colle_profesores.find():
  print(doc)

#coleccion de cursos
colle_cursos =db['cursos']
profe_id      = colle_profesores.insert_one({"nombre":"Andrea Gomez","Profesion":"Ingeniera"}).inserted_id

colle_cursos.insert_one({"nombre":"BigData",
                         "codigo":"UcentraLBig2025",
                         "horarios":[
                             {"dia":"Jueves","hora":"18:00"},
                             {"dia":"Jueves","hora":"20:00"}
                         ],
                         "profesor":profe_id})
     

for doc in colle_cursos.find():
  print(doc)
