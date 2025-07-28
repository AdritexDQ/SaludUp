class DocumentoMedico:

    def __init__(self, tipo, descripcion, archivo, fecha):
        self.tipo = tipo
        self.descripcion = descripcion
        self.archivo = archivo  # En consola solo guardamos como texto
        self.fecha = fecha

    def __str__(self):
        return f"[{self.fecha}] {self.tipo}: {self.descripcion} (Archivo: {self.archivo})"


class Paciente:

    def __init__(self, id_paciente, nombre):
        self.id_paciente = id_paciente
        self.nombre = nombre
        self.documentos = []

    def agregar_documento(self, documento):
        self.documentos.append(documento)

    def listar_documentos(self):
        return self.documentos


class ControlDocumentos:

    def __init__(self):
        self.pacientes = {}

    def obtener_o_crear_paciente(self, id_paciente, nombre=None):
        if id_paciente not in self.pacientes and nombre:
            self.pacientes[id_paciente] = Paciente(id_paciente, nombre)
        return self.pacientes.get(id_paciente)

    def subir_documento(self, paciente, tipo, descripcion, archivo, fecha):
        documento = DocumentoMedico(tipo, descripcion, archivo, fecha)
        paciente.agregar_documento(documento)

    def obtener_documentos(self, paciente):
        return paciente.listar_documentos()


class UIDocumentos:

    def __init__(self, controlador):
        self.controlador = controlador

    def mostrar_menu(self):
        print("\n Sistema de Documentos Médicos")
        id_paciente = input("ID de paciente: ")

        paciente = self.controlador.obtener_o_crear_paciente(id_paciente)

        if not paciente:
            nombre = input("Nombre del paciente: ")
            paciente = self.controlador.obtener_o_crear_paciente(id_paciente, nombre)

        while True:
            print("\n--- Menú ---")
            print("1. Subir nuevo documento")
            print("2. Ver documentos médicos")
            print("3. Salir")
            opcion = input("Seleccione una opción: ")

            if opcion == "1":
                self.subir_documento(paciente)
            elif opcion == "2":
                self.ver_documentos(paciente)
            elif opcion == "3":
                print(" Saliendo del módulo de documentos.")
                break
            else:
                print("❌ Opción inválida.")

    def subir_documento(self, paciente):
        print("\n Subir Documento")
        tipo = input("Tipo de documento (receta, análisis, imagen, etc.): ")
        descripcion = input("Descripción: ")
        archivo = input("Nombre del archivo (simulado): ")
        fecha = input("Fecha del documento (DD-MM-YYYY): ")
        self.controlador.subir_documento(paciente, tipo, descripcion, archivo, fecha)
        print("✅ Documento subido correctamente.")

    def ver_documentos(self, paciente):
        print("\n Documentos del paciente:")
        documentos = self.controlador.obtener_documentos(paciente)
        if documentos:
            for doc in documentos:
                print(f"  - {doc}")
        else:
            print(" No hay documentos registrados.")


if __name__ == "__main__":

    controlador = ControlDocumentos()
    interfaz = UIDocumentos(controlador)
    interfaz.mostrar_menu()
