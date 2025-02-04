# PREDICTOR DE LOTERÍA PERSONALIZADO (Versión Básica)
# Autor: Asistente de OpenAI
# Objetivo: Generar combinaciones basadas en emparejamientos, frecuencia y reducción de dígitos.

import itertools
from collections import defaultdict

def analizar_frecuencia(numeros_historicos):
    frecuencia = defaultdict(int)
    for num in numeros_historicos:
        frecuencia[num] += 1
    return sorted(frecuencia.items(), key=lambda x: x[1], reverse=True)

def encontrar_emparejamientos(numeros_historicos, suma_objetivo=85):
    parejas = []
    for pareja in itertools.combinations(numeros_historicos, 2):
        if sum(pareja) == suma_objetivo:
            parejas.append(pareja)
    return parejas

def reducir_digitos(numero):
    while numero > 9:
        numero = sum(int(digito) for digito in str(numero))
    return numero

def generar_combinacion(numeros_historicos, top_n=10, suma_objetivo=85):
    # Paso 1: Analizar frecuencia
    frecuencia = analizar_frecuencia(numeros_historicos)
    top_numeros = [num for num, _ in frecuencia[:top_n]]
    
    # Paso 2: Encontrar emparejamientos
    parejas = encontrar_emparejamientos(top_numeros, suma_objetivo)
    
    # Paso 3: Reducción de dígitos
    reducciones = [reducir_digitos(num) for num in top_numeros]
    reducciones_frecuentes = list(set([num for num in reducciones if reducciones.count(num) > 1]))
    
    # Generar combinación sugerida
    combinacion = list(set(top_numeros[:6] + reducciones_frecuentes + list(parejas[0]) if parejas else []))
    return combinacion[:6]

# EJEMPLO DE USO CON TUS DATOS
if __name__ == "__main__":
    # Ingresa aquí tus números históricos (ejemplo con datos ficticios)
    numeros_historicos = [
        65, 6, 20, 85, 68, 73, 27, 72, 28, 44,
        89, 43, 34, 51, 7, 52, 6, 3, 22, 75
    ]
    
    # Generar combinación
    combinacion_sugerida = generar_combinacion(numeros_historicos)
    
    # Resultado
    print("\n🔥 Combinación sugerida:")
    print("-" * 30)
    print(" ".join(map(str, sorted(combinacion_sugerida))))
    print("-" * 30)
    print("Frecuencia | Emparejamientos | Reducción de dígitos")
