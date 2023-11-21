import math

def int_to_bytes(n, num_bytes, little_endian=True):
    """Convierte un entero a su representación binaria de manera manual, sin el uso de struct.pack.
       esta función convierte un entero en su representación binaria y permite especificar el número de bytes 
       y el formato (little-endian o big-endian). """
       
    if little_endian:
        return n.to_bytes(num_bytes, 'little', signed=True)
    else:
        return n.to_bytes(num_bytes, 'big', signed=True)

def escalaG(archivo_salida, duracion_por_nota=1.875, frecuencia_muestreo=44100, profundidad_bits=16, num_canales=1, repeticiones=2):
    # Frecuencia base de Sol (G)
    frecuencia_base = 392.0

    # Grados de la escala de Sol
    grados_escala = [0, 2, 4, 5, 7, 9, 11, 12]

    # Calcular el número total de muestras para una nota
    num_muestras_por_nota = int(duracion_por_nota * frecuencia_muestreo)

    # Calcular el número total de muestras para todas las notas
    num_muestras_total = len(grados_escala) * num_muestras_por_nota * repeticiones
    
    """Esta sección se encarga de escribir manualmente el encabezado del archivo WAV en el archivo de salida,
    tambien indicamos los marcadores que especifican que el archivo es tipo RIFF(ASCII), y se escribe byte por byte, 
    el tipo de formato y subsecciones"""

    # Abrir el archivo en modo binario
    with open(archivo_salida, 'wb') as archivo:
        # Escribir el encabezado del archivo WAV manualmente
        archivo.write(b'RIFF')
        archivo.write(int_to_bytes(36 + num_muestras_total * num_canales * profundidad_bits // 8, 4))
        archivo.write(b'WAVE')
        archivo.write(b'fmt ')
        archivo.write(int_to_bytes(16, 4))
        archivo.write(int_to_bytes(1, 2))
        archivo.write(int_to_bytes(num_canales, 2))
        archivo.write(int_to_bytes(frecuencia_muestreo, 4))
        archivo.write(int_to_bytes(frecuencia_muestreo * num_canales * profundidad_bits // 8, 4))
        archivo.write(int_to_bytes(num_canales * profundidad_bits // 8, 2))
        archivo.write(int_to_bytes(profundidad_bits, 2))
        archivo.write(b'data')
        archivo.write(int_to_bytes(num_muestras_total * num_canales * profundidad_bits // 8, 4))
        
        """ Aqui se generan datos de audio para la escala de sol ascendente ahora implementando una repeticion manualmente 
       
        
       Dentro del segundo bucle anidado se itera atraves de cada elemento de la lista, para cada grado se calcula la frecuencia  
        de la nota correspondiente por medio de la formula que se deriva de la relación entre las frecuencias de las notas en una 
        escala musical estándar.
        
        Y cojmo ultimo, en el bucle final se calcula el valor de la muestra. Se utiliza una función sinusoidal para simular la forma de 
        onda de la nota, por medio de su formula general A⋅sin(2πft)"""

        for _ in range(repeticiones):
            
            for grado in grados_escala:
                frecuencia = frecuencia_base * 2 ** (grado / 12.0)
                
                for i in range(num_muestras_por_nota):
                    t = i / frecuencia_muestreo
                    valor = int(32767.0 * 0.5 * math.sin(2 * math.pi * frecuencia * t))
                    archivo.write(int_to_bytes(valor, 2))

"""En esta parte final se elije manualmente la duracion por notas dependiendo de la cantidad de repeticiones que la escala tenga, 
para la frecuencia de muestreo se eligio la mas comun dentro de la musica convencional, se eligieron 16 bits ya que es la calidad estandar 
en produccion musical, un solo canal para salida de audio mono y 2 repeticiones"""

escalaG('escalaG.wav', duracion_por_nota=1.875, frecuencia_muestreo=44100, profundidad_bits=16, num_canales=1, repeticiones=2)
