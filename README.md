#0
ruta = "C:/Users/quiro/Downloads/peliculas.csv"

archivo = open(ruta, "r", encoding="utf-8")
datos = archivo.readlines()
archivo.close()
peliculas = []

for linea in datos:
    print(linea)
    print(linea.split(";"))

#1
def contgen(peliculas):
    gencount = {}
    for pelicula in peliculas:
        generos = pelicula['géneros'].split(',')
        for genero in generos:
            if genero in gencount:
                gencount[genero] += 1
            else:
                gencount[genero] = 1
    return gencount

gencount = contgen(peliculas)
print("Cantidad de películas por género: ")
print(gencount)

#2
def prommedgen(peliculas):
    from statistics import median

    genvotos = {}
    for pelicula in peliculas:
        generos = pelicula['géneros'].split(',')
        promvotos = float(pelicula['promedio de votos'])
        for genero in generos:
            if genero in genvotos:
                genvotos[genero].append(promvotos)
            else:
                genvotos[genero] = [promvotos]

    promemed = {}
    for genero, votos in genvotos.items():
        promemed[genero] = {
            'promedio': sum(votos) / len(votos),
            'mediana': median(votos)
        }
    return promemed

promemed = prommedgen(peliculas)
print("Promedio y mediana del promedio de votos por género: ")
print(promemed)

#3
def desviogen(peliculas):
    genvotos = {}
    for pelicula in peliculas:
        generos = pelicula['géneros'].split(',')
        promvotos = float(pelicula['promedio de votos'])
        for genero in generos:
            if genero in genvotos:
                genvotos[genero].append(promvotos)
            else:
                genvotos[genero] = [promvotos]

    vardes = {}
    for genero, votos in genvotos.items():
        promedio = sum(votos) / len(votos)
        varianza = sum((x - promedio) ** 2 for x in votos) / len(votos)
        desestandar = varianza ** 0.5
        vardes[genero] = {
            'varianza': varianza,
            'desviacion_estandar': desestandar
        }
    return vardes

vardes = desviogen(peliculas)
print("Varianza y desviación estándar del promedio de votos por género: ")
print(vardes)

#4
def mejpelgen(peliculas):
    mejorpeli = {}
    for pelicula in peliculas:
        generos = pelicula['géneros'].split(',')
        promvotos = float(pelicula['promedio de votos'])
        for genero in generos:
            if genero not in mejorpeli or promvotos > mejorpeli[genero]['promedio de votos']:
                mejorpeli[genero] = {
                    'título': pelicula['título'],
                    'promedio de votos': promvotos
                }
    return mejorpeli

mejorpeli = mejpelgen(peliculas)
print("Película con mayor valoración promedio por género: ")
print(mejorpeli)

#5
def top5pelis_genero(peliculas, genero):
    peliculas_genero = [pelicula for pelicula in peliculas if genero in pelicula['géneros']]
    peliculas_genero.sort(key=lambda x: float(x['promedio de votos']), reverse=True)
    return peliculas_genero[:5]

gen = input("Ingrese un género: ")
top5pelis = top5pelis_genero(peliculas, gen)

print(f"Las 5 mejores películas del género {gen} son:")
for pelicula in top5pelis:
    print(pelicula['título'], pelicula['promedio de votos'])

#6
def recomendarporaño(peliculas, año):
    peliaño = [pelicula for pelicula in peliculas if pelicula['fecha de estreno'].startswith(str(año))]
    peliaño.sort(key=lambda x: float(x['promedio de votos']), reverse=True)
    return peliaño[:5]

año = input("Ingrese un año: ")
top5año = recomendarporaño(peliculas, año)

print(f"Las 5 mejores películas del año {año} son:")
for pelicula in top5año:
    print(pelicula['título'], pelicula['promedio de votos'])
