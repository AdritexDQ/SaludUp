class Paciente:

    def __init__(self, nombre, id_paciente):
        self.nombre = nombre
        self.id = id_paciente


class Emergencia:

    def __init__(self, paciente, direccion):
        self.paciente = paciente
        self.direccion = direccion
        self.estado = "enviada"

    def __str__(self):
        return f"Emergencia registrada para {self.paciente.nombre} en {self.direccion}"


class ControlEmergencia:

    def __init__(self):
        self.historial_emergencias = []

    def activar_emergencia(self, paciente, direccion):
        emergencia = Emergencia(paciente, direccion)
        self.historial_emergencias.append(emergencia)
        self.notificar_servicios_emergencia(emergencia)
        return emergencia

    def notificar_servicios_emergencia(self, emergencia):
        print("\n Notificando a servicios de emergencia...")
        print(f"➡ Emergencia para {emergencia.paciente.nombre} en {emergencia.direccion}")
        print("✅ Notificación enviada con éxito.")


class UIEmergencia:

    def __init__(self, controlador):
        self.controlador = controlador

    def mostrar_boton_emergencia(self):
        print("\n Botón de Emergencia")
        nombre = input("Ingrese su nombre: ")
        id_paciente = input("Ingrese su ID de paciente: ")
        direccion = input("Ingrese su dirección actual: ")
        paciente = Paciente(nombre, id_paciente)

        confirmacion = input("¿Desea activar una emergencia? (s/n): ")
        if confirmacion.lower() == "s":
            emergencia = self.controlador.activar_emergencia(paciente, direccion)
            print("✅ Emergencia activada correctamente.")
            print(emergencia)
        else:
            print(" Emergencia cancelada.")


if __name__ == "__main__":

    controlador = ControlEmergencia()
    interfaz = UIEmergencia(controlador)
    interfaz.mostrar_boton_emergencia()
