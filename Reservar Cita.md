class Paciente:

    def __init__(self, nombre, id_paciente):
        self.nombre = nombre
        self.id = id_paciente


class Medico:

    def __init__(self, nombre, especialidad):
        self.nombre = nombre
        self.especialidad = especialidad
        self.horarios_disponibles = ["30-07-2025 10:00", "30-07-2025 15:00"]


class Cita:

    def __init__(self, paciente, medico, fecha_hora):
        self.paciente = paciente
        self.medico = medico
        self.fecha_hora = fecha_hora
        self.estado = "confirmada"

    def __str__(self):
        return f"Cita con el Dr. {self.medico.nombre} el {self.fecha_hora}"


class ControlReservaCita:

    def __init__(self):
        self.medicos = [
            Medico("Juan Pérez", "Cardiología"),
            Medico("Ana Gómez", "Pediatría")
        ]
        self.citas = []

    def mostrar_medicos(self):
        print("\n📋 Médicos disponibles:")
        for i, medico in enumerate(self.medicos):
            print(f"{i + 1}. {medico.nombre} - {medico.especialidad}")

    def obtener_medico(self, indice):
        if 0 <= indice < len(self.medicos):
            return self.medicos[indice]
        return None

    def reservar_cita(self, paciente, medico, fecha_hora):
        if fecha_hora in medico.horarios_disponibles:
            cita = Cita(paciente, medico, fecha_hora)
            self.citas.append(cita)
            medico.horarios_disponibles.remove(fecha_hora)
            return True, cita
        return False, "❌ El horario no está disponible."

class UIReservaCita:

    def __init__(self, controlador):
        self.controlador = controlador

    def iniciar_reserva(self):
        print("\n🩺 Sistema de Reserva de Citas Médicas")
        nombre = input("Ingrese su nombre: ")
        id_paciente = input("Ingrese su ID de paciente: ")
        paciente = Paciente(nombre, id_paciente)

        self.controlador.mostrar_medicos()
        indice = int(input("Seleccione el número del médico: ")) - 1
        medico = self.controlador.obtener_medico(indice)

        if medico:
            print(f"\nHorarios disponibles con {medico.nombre}:")
            for i, horario in enumerate(medico.horarios_disponibles):
                print(f"{i + 1}. {horario}")
            opcion = int(input("Seleccione el número del horario: ")) - 1

            if 0 <= opcion < len(medico.horarios_disponibles):
                horario = medico.horarios_disponibles[opcion]
                ok, resultado = self.controlador.reservar_cita(paciente, medico, horario)
                if ok:
                    print("✅ ¡Cita reservada exitosamente!")
                    print(resultado)
                else:
                    print(resultado)
            else:
                print("❌ Opción de horario no válida.")
        else:
            print("❌ Médico no válido.")



if __name__ == "__main__":

    controlador = ControlReservaCita()
    ui = UIReservaCita(controlador)
    ui.iniciar_reserva()
