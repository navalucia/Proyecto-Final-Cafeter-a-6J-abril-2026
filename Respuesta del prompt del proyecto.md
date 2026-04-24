Esta es una guía técnica estructurada para inicializar tu ecosistema de agentes y desarrollar la aplicación **ProyectoCafeteria**. Utilizaremos una estructura orientada a capacidades (skills) para que tu agente global `.agents` pueda escalar el desarrollo.

---

## 1. Estructura de la Habilidad Agente Global: `.agents`

Primero, definimos la base de conocimiento y operación de tus agentes.

**Estructura de carpetas:**
```text
.agents/
├── SKILL.md              # Definición de capacidades y roles
├── scripts/              # Automatizaciones de despliegue y limpieza
├── ejemplos/             # Snippets de código UI y lógica
└── resources/            # Assets, iconos y temas de color
```

### Contenido de `SKILL.md`
> **Propósito:** Definir el comportamiento del agente para el Proyecto Cafetería.
> * **Skill Diseño:** Responsable de Material Design 3, Temas y UI.
> * **Skill Código:** Lógica de negocio en Dart y gestión de estados.
> * **Skill Scraping:** Obtención de datos de precios de café de competidores.

---

## 2. Prerrequisitos y Configuración del Entorno

Antes de escribir código, debemos asegurar que el motor esté encendido.

### Verificación de Herramientas
Ejecuta estos comandos en tu terminal (VS Code o Antigravity):

1.  **Flutter:** `flutter doctor` (Debe estar instalado).
2.  **Firebase CLI:** Si no lo tienes, instálalo con:
    `npm install -g firebase-tools`
3.  **Flutterfire CLI:** `dart pub global activate flutterfire_cli`
4.  **Login:** `firebase login`

### Vinculación con Firebase
Dentro de tu carpeta `proyectocafeteria`:
```bash
flutterfire configure
```
*Selecciona tu proyecto de Firebase Console y las plataformas (Android/iOS).*

---

## 3. Configuración del Proyecto (`pubspec.yaml`)

Añade las dependencias necesarias para Firestore y la gestión de datos.

```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^3.0.0
  cloud_firestore: ^5.0.0
  cupertino_icons: ^1.0.6
```

---

## 4. Arquitectura de Archivos (CRUD Bebidas)

Generaremos la siguiente estructura para mantener el proyecto limpio:

```text
lib/
├── main.dart
├── models/
│   └── bebida_model.dart
├── services/
│   └── firebase_service.dart
└── screens/
    ├── home_screen.dart
    ├── lista_bebidas.dart
    └── form_bebida.dart
```

---

## 5. Implementación del Código

### A. Modelo de Datos (`models/bebida_model.dart`)
```dart
class Bebida {
  String id;
  String nombre;
  double precio;

  Bebida({required this.id, required this.nombre, required this.precio});

  Map<String, dynamic> toMap() => {"nombre": nombre, "precio": precio};

  factory Bebida.fromFirestore(Map<String, dynamic> data, String id) {
    return Bebida(id: id, nombre: data['nombre'], precio: data['precio'].toDouble());
  }
}
```

### B. Servicio Firestore (`services/firebase_service.dart`)
Aquí es donde el **Skill de Código** hace su magia.

```dart
import 'package:cloud_firestore/cloud_firestore.dart';
import '../models/bebida_model.dart';

class FirebaseService {
  final CollectionReference bebidas = FirebaseFirestore.instance.collection('bebidas');

  // Create
  Future<void> addBebida(String nombre, double precio) {
    return bebidas.add({'nombre': nombre, 'precio': precio});
  }

  // Read
  Stream<List<Bebida>> getBebidas() {
    return bebidas.snapshots().map((snapshot) =>
        snapshot.docs.map((doc) => Bebida.fromFirestore(doc.data() as Map<String, dynamic>, doc.id)).toList());
  }

  // Update
  Future<void> updateBebida(String id, String nuevoNombre, double nuevoPrecio) {
    return bebidas.doc(id).update({'nombre': nuevoNombre, 'precio': nuevoPrecio});
  }

  // Delete
  Future<void> deleteBebida(String id) {
    return bebidas.doc(id).delete();
  }
}
```

### C. Interfaz de Usuario: Pantalla de Lista (`screens/lista_bebidas.dart`)



```dart
import 'package:flutter/material.dart';
import '../services/firebase_service.dart';
import '../models/bebida_model.dart';

class ListaBebidas extends StatelessWidget {
  final FirebaseService _service = FirebaseService();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Menú de Cafetería"), backgroundColor: Colors.brown),
      body: StreamBuilder<List<Bebida>>(
        stream: _service.getBebidas(),
        builder: (context, snapshot) {
          if (!snapshot.hasData) return Center(child: CircularProgressIndicator());
          return ListView.builder(
            itemCount: snapshot.data!.length,
            itemBuilder: (context, index) {
              Bebida b = snapshot.data![index];
              return ListTile(
                title: Text(b.nombre),
                subtitle: Text("\$${b.precio}"),
                trailing: IconButton(
                  icon: Icon(Icons.delete, color: Colors.red),
                  onPressed: () => _service.deleteBebida(b.id),
                ),
                onTap: () => _mostrarDialogoEdicion(context, b),
              );
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        child: Icon(Icons.add),
        onPressed: () => _mostrarDialogoEdicion(context, null),
      ),
    );
  }

  // Dialogo simplificado para Crear/Editar
  void _mostrarDialogoEdicion(BuildContext context, Bebida? bebida) {
    // Implementar lógica de controladores de texto aquí
  }
}
```

---

## 6. Punto de Entrada (`main.dart`)

```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'screens/lista_bebidas.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(CafeteriaApp());
}

class CafeteriaApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Cafetería Agente',
      theme: ThemeData(primarySwatch: Colors.brown, useMaterial3: true),
      home: ListaBebidas(),
    );
  }
}
```

---

### Pasos finales para la activación de ".agents":
1.  **Validación:** Ejecuta `flutter run` para verificar la conexión con Firestore.
2.  **Sincronización:** Usa el **Skill de Diseño** para ajustar los colores `resources/` a un tono café artesanal.
3.  **Logs:** Los errores de conexión se reflejarán en la terminal de tu IDE (Antigravity/VS Code).

¿Deseas que profundice en el código del **Skill de Scraping** para comparar precios de granos de café automáticamente?
