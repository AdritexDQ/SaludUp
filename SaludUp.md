class Usuario:

    def __init__(self, correo, nombre, contrasena):
        self.correo = correo
        self.nombre = nombre
        self.contrasena = contrasena


class Paciente(Usuario):

    def __init__(self, correo, nombre, contrasena, id_paciente):
        super().__init__(correo, nombre, contrasena)
        self.id_paciente = id_paciente


class Cita:

    def __init__(self, paciente, especialidad, fecha, hora):
        self.paciente = paciente
        self.especialidad = especialidad
        self.fecha = fecha
        self.hora = hora


class Diagnostico:

    def __init__(self, descripcion, fecha):
        self.descripcion = descripcion
        self.fecha = fecha


class Receta:

    def __init__(self, medicamentos, fecha):
        self.medicamentos = medicamentos
        self.fecha = fecha


class DocumentoMedico:

    def __init__(self, tipo, descripcion, archivo, fecha):
        self.tipo = tipo
        self.descripcion = descripcion
        self.archivo = archivo
        self.fecha = fecha


class Emergencia:

    def __init__(self, paciente, direccion):
        self.paciente = paciente
        self.direccion = direccion
        self.estado = "enviada"


class CentroSalud:

    def __init__(self, nombre, tipo, direccion, horario, telefono):
        self.nombre = nombre
        self.tipo = tipo
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

class ControlUsuarios:

    def __init__(self):
        self.usuarios = {}

    def registrar_usuario(self, correo, nombre, contrasena):
        if correo in self.usuarios:
            return False  # ya existe
        self.usuarios[correo] = Usuario(correo, nombre, contrasena)
        return True

    def iniciar_sesion(self, correo, contrasena):
        usuario = self.usuarios.get(correo)
        if usuario and usuario.contrasena == contrasena:
            return usuario
        return None


class ControlCitas:

    def __init__(self):
        self.citas = []

    def reservar_cita(self, paciente, especialidad, fecha, hora):
        nueva_cita = Cita(paciente, especialidad, fecha, hora)
        self.citas.append(nueva_cita)
        return nueva_cita

    def listar_citas_paciente(self, id_paciente):
        return [c for c in self.citas if c.paciente.id_paciente == id_paciente]


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


class ControlHistorial:

    def __init__(self):
        self.historiales = {}

    def obtener_o_crear_historial(self, id_paciente):
        if id_paciente not in self.historiales:
            self.historiales[id_paciente] = HistorialMedico(id_paciente)
        return self.historiales[id_paciente]

    def agregar_diagnostico(self, id_paciente, descripcion, fecha):
        historial = self.obtener_o_crear_historial(id_paciente)
        historial.agregar_diagnostico(Diagnostico(descripcion, fecha))

    def agregar_receta(self, id_paciente, medicamentos, fecha):
        historial = self.obtener_o_crear_historial(id_paciente)
        historial.agregar_receta(Receta(medicamentos, fecha))


class ControlDocumentos:

    def __init__(self):
        self.documentos = {}  # clave: id_paciente

    def subir_documento(self, id_paciente, tipo, descripcion, archivo, fecha):
        doc = DocumentoMedico(tipo, descripcion, archivo, fecha)
        if id_paciente not in self.documentos:
            self.documentos[id_paciente] = []
        self.documentos[id_paciente].append(doc)

    def obtener_documentos(self, id_paciente):
        return self.documentos.get(id_paciente, [])


class ControlEmergencia:

    def __init__(self):
        self.historial_emergencias = []

    def activar_emergencia(self, paciente, direccion):
        emergencia = Emergencia(paciente, direccion)
        self.historial_emergencias.append(emergencia)
        self.notificar_servicios(emergencia)
        return emergencia

    def notificar_servicios(self, emergencia):
        print("\n Notificando a servicios de emergencia...")
        print(f" Emergencia para {emergencia.paciente.nombre} en {emergencia.direccion}")
        print("✅ Notificación enviada con éxito.")


class ControlUsuarios:

    def __init__(self):
        self.usuarios = {}

    def registrar_usuario(self, correo, nombre, contrasena):
        if correo in self.usuarios:
            return False  # ya existe
        self.usuarios[correo] = Usuario(correo, nombre, contrasena)
        return True

    def iniciar_sesion(self, correo, contrasena):
        usuario = self.usuarios.get(correo)
        if usuario and usuario.contrasena == contrasena:
            return usuario
        return None


class ControlCitas:

    def __init__(self):
        self.citas = []

    def reservar_cita(self, paciente, especialidad, fecha, hora):
        nueva_cita = Cita(paciente, especialidad, fecha, hora)
        self.citas.append(nueva_cita)
        return nueva_cita

    def listar_citas_paciente(self, id_paciente):
        return [c for c in self.citas if c.paciente.id_paciente == id_paciente]


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


class ControlHistorial:

    def __init__(self):
        self.historiales = {}

    def obtener_o_crear_historial(self, id_paciente):
        if id_paciente not in self.historiales:
            self.historiales[id_paciente] = HistorialMedico(id_paciente)
        return self.historiales[id_paciente]

    def agregar_diagnostico(self, id_paciente, descripcion, fecha):
        historial = self.obtener_o_crear_historial(id_paciente)
        historial.agregar_diagnostico(Diagnostico(descripcion, fecha))

    def agregar_receta(self, id_paciente, medicamentos, fecha):
        historial = self.obtener_o_crear_historial(id_paciente)
        historial.agregar_receta(Receta(medicamentos, fecha))


class ControlDocumentos:

    def __init__(self):
        self.documentos = {}  # clave: id_paciente

    def subir_documento(self, id_paciente, tipo, descripcion, archivo, fecha):
        doc = DocumentoMedico(tipo, descripcion, archivo, fecha)
        if id_paciente not in self.documentos:
            self.documentos[id_paciente] = []
        self.documentos[id_paciente].append(doc)

    def obtener_documentos(self, id_paciente):
        return self.documentos.get(id_paciente, [])


class ControlEmergencia:

    def __init__(self):
        self.historial_emergencias = []

    def activar_emergencia(self, paciente, direccion):
        emergencia = Emergencia(paciente, direccion)
        self.historial_emergencias.append(emergencia)
        self.notificar_servicios(emergencia)
        return emergencia

    def notificar_servicios(self, emergencia):
        print("\n Notificando a servicios de emergencia...")
        print(f" Emergencia para {emergencia.paciente.nombre} en {emergencia.direccion}")
        print("✅ Notificación enviada con éxito.")


class ControlUsuarios:

    def __init__(self):
        self.usuarios = {}

    def registrar_usuario(self, correo, nombre, contrasena):
        if correo in self.usuarios:
            return False  # ya existe
        self.usuarios[correo] = Usuario(correo, nombre, contrasena)
        return True

    def iniciar_sesion(self, correo, contrasena):
        usuario = self.usuarios.get(correo)
        if usuario and usuario.contrasena == contrasena:
            return usuario
        return None


class ControlCitas:

    def __init__(self):
        self.citas = []

    def reservar_cita(self, paciente, especialidad, fecha, hora):
        nueva_cita = Cita(paciente, especialidad, fecha, hora)
        self.citas.append(nueva_cita)
        return nueva_cita

    def listar_citas_paciente(self, id_paciente):
        return [c for c in self.citas if c.paciente.id_paciente == id_paciente]


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


class ControlHistorial:

    def __init__(self):
        self.historiales = {}

    def obtener_o_crear_historial(self, id_paciente):
        if id_paciente not in self.historiales:
            self.historiales[id_paciente] = HistorialMedico(id_paciente)
        return self.historiales[id_paciente]

    def agregar_diagnostico(self, id_paciente, descripcion, fecha):
        historial = self.obtener_o_crear_historial(id_paciente)
        historial.agregar_diagnostico(Diagnostico(descripcion, fecha))

    def agregar_receta(self, id_paciente, medicamentos, fecha):
        historial = self.obtener_o_crear_historial(id_paciente)
        historial.agregar_receta(Receta(medicamentos, fecha))


class ControlDocumentos:

    def __init__(self):
        self.documentos = {}  # clave: id_paciente

    def subir_documento(self, id_paciente, tipo, descripcion, archivo, fecha):
        doc = DocumentoMedico(tipo, descripcion, archivo, fecha)
        if id_paciente not in self.documentos:
            self.documentos[id_paciente] = []
        self.documentos[id_paciente].append(doc)

    def obtener_documentos(self, id_paciente):
        return self.documentos.get(id_paciente, [])


class ControlEmergencia:

    def __init__(self):
        self.historial_emergencias = []

    def activar_emergencia(self, paciente, direccion):
        emergencia = Emergencia(paciente, direccion)
        self.historial_emergencias.append(emergencia)
        self.notificar_servicios(emergencia)
        return emergencia

    def notificar_servicios(self, emergencia):
        print("\n Notificando a servicios de emergencia...")
        print(f" Emergencia para {emergencia.paciente.nombre} en {emergencia.direccion}")
        print("✅ Notificación enviada con éxito.")


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

    def obtener_todos(self):
        return self.centros


class UIMenuPrincipal:

    def __init__(self, ui_usuario, ui_citas, ui_historial, ui_documentos, ui_emergencia, ui_centros):
        self.ui_usuario = ui_usuario
        self.ui_citas = ui_citas
        self.ui_historial = ui_historial
        self.ui_documentos = ui_documentos
        self.ui_emergencia = ui_emergencia
        self.ui_centros = ui_centros
        self.usuario_actual = None

    def ejecutar(self):
        print(" Bienvenido a SaludUp")
        self.usuario_actual = self.ui_usuario.menu_inicio_sesion()

        if self.usuario_actual:
            while True:
                print(f"\n Usuario: {self.usuario_actual.nombre}")
                print("--- Menú Principal ---")
                print("1. Reservar Cita Médica")
                print("2. Ver Historial Médico")
                print("3. Subir / Ver Documentos Médicos")
                print("4. Botón de Emergencia")
                print("5. Ver Centros de Salud")
                print("6. Salir")

                opcion = input("Seleccione una opción: ")
                if opcion == "1":
                    self.ui_citas.menu_citas(self.usuario_actual)
                elif opcion == "2":
                    self.ui_historial.menu_historial(self.usuario_actual)
                elif opcion == "3":
                    self.ui_documentos.menu_documentos(self.usuario_actual)
                elif opcion == "4":
                    self.ui_emergencia.activar_emergencia(self.usuario_actual)
                elif opcion == "5":
                    self.ui_centros.menu_centros()
                elif opcion == "6":
                    print(" Gracias por usar SaludUp. ¡Hasta pronto!")
                    break
                else:
                    print(" Opción no válida.")


class UIUsuario:

    def __init__(self, controlador):
        self.controlador = controlador

    def menu_inicio_sesion(self):
        while True:
            print("\n--- Inicio de Sesión / Registro ---")
            print("1. Registrarse")
            print("2. Iniciar sesión")
            print("3. Salir")
            opcion = input("Seleccione una opción: ")

            if opcion == "1":
                correo = input("Correo: ")
                nombre = input("Nombre: ")
                contrasena = input("Contraseña: ")
                if self.controlador.registrar_usuario(correo, nombre, contrasena):
                    print("✅ Registro exitoso.")
                else:
                    print("❌ Ese correo ya está registrado.")
            elif opcion == "2":
                correo = input("Correo: ")
                contrasena = input("Contraseña: ")
                usuario = self.controlador.iniciar_sesion(correo, contrasena)
                if usuario:
                    print(f"✅ Bienvenido {usuario.nombre}")
                    return Paciente(correo, usuario.nombre, contrasena, id_paciente=correo)
                else:
                    print("❌ Credenciales incorrectas.")
            elif opcion == "3":
                return None
            else:
                print(" Opción inválida.")


class UICitas:

    def __init__(self, controlador):
        self.controlador = controlador

    def menu_citas(self, paciente):
        print("\n--- Reserva de Cita Médica ---")
        especialidad = input("Especialidad: ")
        fecha = input("Fecha (DD-MM-YYYY): ")
        hora = input("Hora (HH:MM): ")
        cita = self.controlador.reservar_cita(paciente, especialidad, fecha, hora)
        print(f"✅ Cita reservada para {cita.fecha} a las {cita.hora} con {cita.especialidad}.")


class UIHistorial:

    def __init__(self, controlador):
        self.controlador = controlador

    def menu_historial(self, paciente):
        historial = self.controlador.obtener_o_crear_historial(paciente.id_paciente)
        while True:
            print("\n--- Historial Médico ---")
            print("1. Ver todo el historial")
            print("2. Ver diagnósticos")
            print("3. Ver recetas")
            print("4. Agregar diagnóstico")
            print("5. Agregar receta")
            print("6. Volver")
            opcion = input("Seleccione una opción: ")

            if opcion == "1":
                self.mostrar_diagnosticos(historial)
                self.mostrar_recetas(historial)
            elif opcion == "2":
                self.mostrar_diagnosticos(historial)
            elif opcion == "3":
                self.mostrar_recetas(historial)
            elif opcion == "4":
                desc = input("Descripción del diagnóstico: ")
                fecha = input("Fecha (DD-MM-YYYY): ")
                self.controlador.agregar_diagnostico(paciente.id_paciente, desc, fecha)
            elif opcion == "5":
                meds = input("Medicamentos (separados por coma): ").split(",")
                fecha = input("Fecha (DD-MM-YYYY): ")
                self.controlador.agregar_receta(paciente.id_paciente, [m.strip() for m in meds], fecha)
            elif opcion == "6":
                break
            else:
                print(" Opción inválida.")

    def mostrar_diagnosticos(self, historial):
        print("\n Diagnósticos:")
        for d in historial.listar_diagnosticos():
            print(f"  - {d}")

    def mostrar_recetas(self, historial):
        print("\n Recetas:")
        for r in historial.listar_recetas():
            print(f"  - {r}")


class UIDocumentos:

    def __init__(self, controlador):
        self.controlador = controlador

    def menu_documentos(self, paciente):
        while True:
            print("\n--- Documentos Médicos ---")
            print("1. Subir nuevo documento")
            print("2. Ver documentos")
            print("3. Volver")
            opcion = input("Seleccione una opción: ")

            if opcion == "1":
                tipo = input("Tipo de documento: ")
                descripcion = input("Descripción: ")
                archivo = input("Nombre de archivo (ej. informe.pdf): ")
                fecha = input("Fecha (DD-MM-YYYY): ")
                self.controlador.subir_documento(paciente.id_paciente, tipo, descripcion, archivo, fecha)
                print("✅ Documento subido.")
            elif opcion == "2":
                docs = self.controlador.obtener_documentos(paciente.id_paciente)
                if not docs:
                    print(" No hay documentos.")
                else:
                    for doc in docs:
                        print(f" [{doc.fecha}] {doc.tipo} - {doc.descripcion} ({doc.archivo})")
            elif opcion == "3":
                break
            else:
                print(" Opción inválida.")


class UIEmergencia:

    def __init__(self, controlador):
        self.controlador = controlador

    def activar_emergencia(self, paciente):
        print("\n Activación de Botón de Emergencia")
        direccion = input("Ingrese su dirección actual: ")
        emergencia = self.controlador.activar_emergencia(paciente, direccion)
        print("✅ Emergencia registrada y notificada.")


class UICentros:

    def __init__(self, controlador):
        self.controlador = controlador

    def menu_centros(self):
        while True:
            print("\n--- Centros de Salud ---")
            print("1. Ver todos")
            print("2. Buscar por tipo")
            print("3. Volver")
            opcion = input("Seleccione una opción: ")

            if opcion == "1":
                for centro in self.controlador.obtener_todos():
                    print(f"\n{centro}")
            elif opcion == "2":
                tipo = input("Tipo (hospital, clínica, posta): ")
                encontrados = self.controlador.buscar_centros_por_tipo(tipo)
                if not encontrados:
                    print(" No se encontraron centros.")
                else:
                    for centro in encontrados:
                        print(f"\n{centro}")
            elif opcion == "3":
                break
            else:
                print(" Opción inválida.")


def main():

    # Crear controladores
    control_usuarios = ControlUsuarios()
    control_citas = ControlCitas()
    control_historial = ControlHistorial()
    control_documentos = ControlDocumentos()
    control_emergencia = ControlEmergencia()
    control_centros = ControlCentros()

    # Crear interfaces de usuario
    ui_usuario = UIUsuario(control_usuarios)
    ui_citas = UICitas(control_citas)
    ui_historial = UIHistorial(control_historial)
    ui_documentos = UIDocumentos(control_documentos)
    ui_emergencia = UIEmergencia(control_emergencia)
    ui_centros = UICentros(control_centros)

    # Menú principal
    menu = UIMenuPrincipal(
        ui_usuario, ui_citas, ui_historial,
        ui_documentos, ui_emergencia, ui_centros
    )
    menu.ejecutar()


if __name__ == "__main__":

    main()
