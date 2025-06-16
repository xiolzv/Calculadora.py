# Calculadora.py

import falcon
import calculadora
import json

class OperacionesResource:
    def on_post(self, req, resp, operacion):
        data = json.load(req.bounded_stream)
        if operacion in ["sumar", "restar", "multiplicar", "dividir"]:
            a = data.get("a")
            b = data.get("b")
            resultado = getattr(calculadora, operacion)(a, b)
        elif operacion in ["es_par", "es_impar", "es_primo"]:
            numero = data.get("numero")
            resultado = getattr(calculadora, operacion)(numero)
        elif operacion in ["mayusculas", "minusculas", "title"]:
            cadena = data.get("cadena")
            nombre_func = {
                "mayusculas": "convertir_a_mayusculas",
                "minusculas": "convertir_a_minusculas",
                "title": "convertir_a_title"
            }[operacion]
            resultado = getattr(calculadora, nombre_func)(cadena)
        else:
            resp.status = falcon.HTTP_404
            resultado = "Operación no encontrada"
        resp.media = {"resultado": resultado}

app = falcon.App()
app.add_route('/{operacion}', OperacionesResource())
