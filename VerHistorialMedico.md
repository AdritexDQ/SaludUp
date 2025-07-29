class Diagnostico:

    def __init__(self, descripcion, fecha):
        self.descripcion = descripcion
        self.fecha = fecha

    def __str__(self):
        return f"[{self.fecha}] Diagnóstico: {self.descripcion}"


class Receta:

    def __init__(self, medicamentos, fecha):
        self.medicamentos = medicamentos
        self.fecha = fecha

    def __str__(self):
        return f"[{self.fecha}] Receta: {', '.join(self.medicamentos)}"


class HistorialMedico:

    def __init__(self, id_paciente):
        self.id_paciente = id_paciente
        self.diagnosticos = []
        self.recetas = []

    def agregar_diagnostico(self, diagnostico):
        self.diagnosticos.append(diagnostico)

    def agregar_receta(self, receta):
        self.recetas.append(receta)

    def listar_diagnosticos(self):
        return self.diagnosticos

    def listar_recetas(self):
        return self.recetas


class ControlConsultaHistorial:

    def __init__(self):
        self.historiales = {}  # clave: id_paciente

    def obtener_o_crear_historial(self, id_paciente):
        if id_paciente not in self.historiales:
            self.historiales[id_paciente] = HistorialMedico(id_paciente)
        return self.historiales[id_paciente]

    def agregar_diagnostico(self, id_paciente, descripcion, fecha):
        historial = self.obtener_o_crear_historial(id_paciente)
        diagnostico = Diagnostico(descripcion, fecha)
        historial.agregar_diagnostico(diagnostico)

    def agregar_receta(self, id_paciente, medicamentos, fecha):
        historial = self.obtener_o_crear_historial(id_paciente)
        receta = Receta(medicamentos, fecha)
        historial.agregar_receta(receta)

    def obtener_historial(self, id_paciente):
        return self.historiales.get(id_paciente)



class UIHistorial:

    def __init__(self, controlador):
        self.controlador = controlador

    def ejecutar(self):
        print("\n Bienvenido al Historial Médico de SaludUp")
        id_paciente = input("Ingrese su ID de paciente: ")
        historial = self.controlador.obtener_o_crear_historial(id_paciente)

        while True:
            print("\n--- Menú del Historial Médico ---")
            print("1. Ver todo el historial")
            print("2. Ver solo diagnósticos")
            print("3. Ver solo recetas")
            print("4. Agregar diagnóstico")
            print("5. Agregar receta")
            print("6. Salir")
            opcion = input("Seleccione una opción: ")

            if opcion == "1":
                self.mostrar_historial_completo(historial)
            elif opcion == "2":
                self.mostrar_diagnosticos(historial)
            elif opcion == "3":
                self.mostrar_recetas(historial)
            elif opcion == "4":
                self.agregar_diagnostico(id_paciente)
            elif opcion == "5":
                self.agregar_receta(id_paciente)
            elif opcion == "6":
                print(" Saliendo del historial médico...")
                break
            else:
                print(" Opción inválida.")

    def mostrar_diagnosticos(self, historial):
        print("\n Diagnósticos:")
        for d in historial.listar_diagnosticos():
            print(f"  - {d}")
        if not historial.diagnosticos:
            print("  (No hay diagnósticos registrados)")

    def mostrar_recetas(self, historial):
        print("\n Recetas:")
        for r in historial.listar_recetas():
            print(f"  - {r}")
        if not historial.recetas:
            print("  (No hay recetas registradas)")

    def mostrar_historial_completo(self, historial):
        self.mostrar_diagnosticos(historial)
        self.mostrar_recetas(historial)

    def agregar_diagnostico(self, id_paciente):
        print("\n Agregar Diagnóstico")
        descripcion = input("Descripción del diagnóstico: ")
        fecha = input("Fecha (DD-MM-YYYY): ")
        self.controlador.agregar_diagnostico(id_paciente, descripcion, fecha)
        print("✅ Diagnóstico agregado correctamente.")

    def agregar_receta(self, id_paciente):
        print("\n Agregar Receta")
        medicamentos = input("Ingrese los medicamentos separados por comas: ")
        lista_meds = [med.strip() for med in medicamentos.split(",")]
        fecha = input("Fecha (YYYY-MM-DD): ")
        self.controlador.agregar_receta(id_paciente, lista_meds, fecha)
        print("✅ Receta agregada correctamente.")


if __name__ == "__main__":

    controlador = ControlConsultaHistorial()
    interfaz = UIHistorial(controlador)
    interfaz.ejecutar()
