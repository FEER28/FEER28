class Nodo:
    def __init__(self, vuelo_id, origen, destino, hora_salida, ocupacion):
        self.vuelo_id = vuelo_id  # Número de vuelo
        self.origen = origen  # Ciudad de origen
        self.destino = destino  # Ciudad de destino
        self.hora_salida = hora_salida  # Hora de salida
        self.ocupacion = ocupacion  # Porcentaje de ocupación
        self.izquierda = None  # Hijo izquierdo
        self.derecha = None  # Hijo derecho


class ArbolBinario:
    def __init__(self):
        self.raiz = None

    # Inserción de un nuevo vuelo en el árbol binario
    def insertar(self, nodo, vuelo_id, origen, destino, hora_salida, ocupacion):
        if nodo is None:
            return Nodo(vuelo_id, origen, destino, hora_salida, ocupacion)
        
        if vuelo_id < nodo.vuelo_id:  # Si el número de vuelo es menor, ir al subárbol izquierdo
            nodo.izquierda = self.insertar(nodo.izquierda, vuelo_id, origen, destino, hora_salida, ocupacion)
        else:  # Si el número de vuelo es mayor, ir al subárbol derecho
            nodo.derecha = self.insertar(nodo.derecha, vuelo_id, origen, destino, hora_salida, ocupacion)
        
        return nodo

    # Búsqueda de un vuelo por su ID
    def buscar(self, nodo, vuelo_id):
        if nodo is None or nodo.vuelo_id == vuelo_id:
            return nodo
        
        if vuelo_id < nodo.vuelo_id:
            return self.buscar(nodo.izquierda, vuelo_id)
        
        return self.buscar(nodo.derecha, vuelo_id)

    # Eliminación de un vuelo del árbol
    def eliminar(self, nodo, vuelo_id):
        if nodo is None:
            return nodo
        
        if vuelo_id < nodo.vuelo_id:
            nodo.izquierda = self.eliminar(nodo.izquierda, vuelo_id)
        elif vuelo_id > nodo.vuelo_id:
            nodo.derecha = self.eliminar(nodo.derecha, vuelo_id)
        else:
            # Caso 1: El nodo tiene un solo hijo o no tiene hijos
            if nodo.izquierda is None:
                return nodo.derecha
            elif nodo.derecha is None:
                return nodo.izquierda
            
            # Caso 2: El nodo tiene dos hijos
            # Buscar el sucesor (el nodo más pequeño en el subárbol derecho)
            sucesor = self._min_value_node(nodo.derecha)
            nodo.vuelo_id = sucesor.vuelo_id
            nodo.origen = sucesor.origen
            nodo.destino = sucesor.destino
            nodo.hora_salida = sucesor.hora_salida
            nodo.ocupacion = sucesor.ocupacion
            nodo.derecha = self.eliminar(nodo.derecha, sucesor.vuelo_id)
        
        return nodo

    # Función auxiliar para encontrar el nodo con el valor mínimo (más a la izquierda)
    def _min_value_node(self, nodo):
        actual = nodo
        while actual.izquierda is not None:
            actual = actual.izquierda
        return actual

    # Recorrido en orden para mostrar los vuelos en orden ascendente
    def recorrer_en_orden(self, nodo):
        if nodo:
            self.recorrer_en_orden(nodo.izquierda)
            print(f"Vuelo ID: {nodo.vuelo_id}, Origen: {nodo.origen}, Destino: {nodo.destino}, Hora de Salida: {nodo.hora_salida}, Ocupación: {nodo.ocupacion}%")
            self.recorrer_en_orden(nodo.derecha)

    # Recorrido preorden (mostrar el vuelo actual antes de sus subárboles)
    def recorrer_preorden(self, nodo):
        if nodo:
            print(f"Vuelo ID: {nodo.vuelo_id}, Origen: {nodo.origen}, Destino: {nodo.destino}, Hora de Salida: {nodo.hora_salida}, Ocupación: {nodo.ocupacion}%")
            self.recorrer_preorden(nodo.izquierda)
            self.recorrer_preorden(nodo.derecha)

    # Recorrido postorden (mostrar los vuelos después de sus subárboles)
    def recorrer_postorden(self, nodo):
        if nodo:
            self.recorrer_postorden(nodo.izquierda)
            self.recorrer_postorden(nodo.derecha)
            print(f"Vuelo ID: {nodo.vuelo_id}, Origen: {nodo.origen}, Destino: {nodo.destino}, Hora de Salida: {nodo.hora_salida}, Ocupación: {nodo.ocupacion}%")
