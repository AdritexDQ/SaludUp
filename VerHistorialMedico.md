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


class ControlConsultaHistorial:

    def __init__(self):
        self.historiales = {}  # clave: id_paciente

    def crear_historial_demo(self, id_paciente):
        historial = HistorialMedico(id_paciente)
        historial.agregar_diagnostico(Diagnostico("Gripe común", "30-07-2025"))
        historial.agregar_diagnostico(Diagnostico("Alergia estacional", "30-07-2025"))
        historial.agregar_receta(Receta(["Paracetamol", "Antihistamínico"], "30-07-2025"))
        self.historiales[id_paciente] = historial

    def obtener_historial(self, id_paciente):
        return self.historiales.get(id_paciente, None)


class UIHistorial:

    def __init__(self, controlador):
        self.controlador = controlador

    def ver_historial(self):
        print("\n Consulta de Historial Médico")
        id_paciente = input("Ingrese su ID de paciente: ")
        
        historial = self.controlador.obtener_historial(id_paciente)
        if historial:
            print(f"\n Historial médico del paciente {id_paciente}")
            print("\n Diagnósticos:")
            for d in historial.diagnosticos:
                print(f"  - {d}")
            print("\n Recetas:")
            for r in historial.recetas:
                print(f"  - {r}")
        else:
            print(" No se encontró historial médico para ese ID.")


if __name__ == "__main__":

    controlador = ControlConsultaHistorial()
    # Crear un historial de demostración
    controlador.crear_historial_demo("1234")
    
    interfaz = UIHistorial(controlador)
    interfaz.ver_historial()
