# Proceso MongoDB en Linux

## Iniciar MongoDB

### Iniciar el servicio
sudo systemctl start mongodb

### Habilitarlo para que inicie automáticamente al arrancar
sudo systemctl enable mongodb

### Verificar que está corriendo
sudo systemctl status mongodb

## Script para importar los data sets:

```{bash}
cd mongodb-sample-dataset

./script.sh

sudo lsof -i :27017
```

*Si da errores, o haces cambios, debes reiniciar mongodb*
*o no arrancará.*

Posteriormente entramos ejecutando: 

```{bash}
mongosh

test> show databases
admin                40.00 KiB
config               12.00 KiB
local                72.00 KiB
sample_airbnb        52.12 MiB
sample_analytics      9.38 MiB
sample_geospatial   796.00 KiB
sample_mflix         28.42 MiB
sample_supplies     968.00 KiB
sample_training      61.21 MiB
sample_weatherdata    2.56 MiB

test> use sample_training
```

## Ejercicios

### Ejercicio 1

**En sample_training.zips ¿Cuántas colecciones tienen**
**menos de 1000 personas en el campo pop?**

```{bash}
db.zips.countDocuments({ pop: { $lt: 1000 } })
```

Resultado: 8065

### Ejercicio 2

**En sample_training.trips ¿Cuál es la diferencia entre la**
**gente que nació en 1998 y la que nació después de 1998?**

```{bash}
db.trips.countDocuments({ "birth year": 1998 }) - db.trips.countDocuments({ "birth year": { $gt: 1998 } })
```

Resultado: -6

### Ejercicio 3

**En sample_training.routes ¿Cuántas rutas tienen al menos**
**una parada?**

```{bash}
db.routes.countDocuments({ stops: { $gte: 1 } })
```

Resultado: 11

### Ejercicio 4

```{bash}
db.inspections.countDocuments({
  result: "Out of Business",
  sector: "Home Improvement Contractor - 100"
})
```

Resultado: 4

### Ejercicio 5

```{bash}
db.inspections.countDocuments({
  $and: [
    { date: { $in: ["Feb 20 2015", "Feb 21 2015"] } },
    { sector: { $ne: "Cigarette Retail Dealer - 127" } }
  ]
})
```

Resultado: 204