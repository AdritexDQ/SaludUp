class CentroSalud:

    def __init__(self, nombre, tipo, direccion, horario, telefono):
        self.nombre = nombre
        self.tipo = tipo  # Hospital, Clínica, Posta, etc.
        self.direccion = direccion
        self.horario = horario
        self.telefono = telefono

    def __str__(self):
        return (
            f" {self.nombre} ({self.tipo})\n"
            f" Dirección: {self.direccion}\n"
            f" Horario: {self.horario}\n"
            f" Teléfono: {self.telefono}"
        )


class ControlCentros:

    def __init__(self):
        self.centros = []
        self._cargar_centros_demo()

    def _cargar_centros_demo(self):
        self.centros.append(CentroSalud(
            "Clínica Central", "Clínica",
            "Av. Bolívar #1450", "08:00 - 20:00", "44567890"
        ))
        self.centros.append(CentroSalud(
            "Hospital Cochabamba", "Hospital",
            "Av. I. Montes #1200", "24 horas", "44567999"
        ))
        self.centros.append(CentroSalud(
            "Posta San Pedro", "Posta",
            "Calle San Pedro #320", "07:00 - 15:00", "44567001"
        ))

    def buscar_centros_por_tipo(self, tipo):
        return [c for c in self.centros if c.tipo.lower() == tipo.lower()]

    def obtener_todos_los_centros(self):
        return self.centros


class UICentros:

    def __init__(self, controlador):
        self.controlador = controlador

    def mostrar_menu(self):
        print("\n Consulta de Centros de Salud")

        while True:
            print("\n--- Menú ---")
            print("1. Ver todos los centros de salud")
            print("2. Buscar por tipo (hospital, clínica, etc.)")
            print("3. Salir")
            opcion = input("Seleccione una opción: ")

            if opcion == "1":
                self.mostrar_todos()
            elif opcion == "2":
                self.buscar_por_tipo()
            elif opcion == "3":
                print(" Saliendo del módulo de centros de salud...")
                break
            else:
                print("❌ Opción inválida.")

    def mostrar_todos(self):
        centros = self.controlador.obtener_todos_los_centros()
        print("\n Lista de Centros de Salud:")
        for centro in centros:
            print("\n" + str(centro))

    def buscar_por_tipo(self):
        tipo = input("Ingrese el tipo de centro (ej. clínica, hospital, posta): ")
        resultados = self.controlador.buscar_centros_por_tipo(tipo)
        if resultados:
            print(f"\n Resultados para tipo '{tipo}':")
            for centro in resultados:
                print("\n" + str(centro))
        else:
            print(" No se encontraron centros de ese tipo.")


if __name__ == "__main__":

    controlador = ControlCentros()
    interfaz = UICentros(controlador)
    interfaz.mostrar_menu()
