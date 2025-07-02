import React, { useState, useEffect, createContext, useContext } from 'react';
import { initializeApp } from 'firebase/app';
import { getAuth, signInWithEmailAndPassword, onAuthStateChanged, signInAnonymously, signOut } from 'firebase/auth';
import { getFirestore, collection, addDoc, query, where, onSnapshot, doc, getDoc, updateDoc, deleteDoc } from 'firebase/firestore';
import { getStorage, ref, uploadBytes, getDownloadURL } from 'firebase/storage';

// Contexto de Autenticación
const AuthContext = createContext(null);

// Componente de Mensajes
const MessageBox = ({ message, type, onClose }) => {
    if (!message) return null;
    return (
        <div className={`fixed top-4 left-1/2 -translate-x-1/2 p-4 rounded-lg shadow-lg z-50 ${
            type === 'success' ? 'bg-green-500 text-white' : 'bg-red-500 text-white'
        } flex items-center justify-between animate-fade-in-down`}>
            <span>{message}</span>
            <button onClick={onClose} className="ml-4 text-lg font-bold">&times;</button>
        </div>
    );
};

// Componente de Carga
const LoadingSpinner = () => (
    <div className="fixed inset-0 bg-gray-800 bg-opacity-75 flex items-center justify-center z-[100]">
        <div className="animate-spin rounded-full h-16 w-16 border-t-4 border-b-4 border-blue-500"></div>
        <p className="ml-4 text-white text-lg">Cargando...</p>
    </div>
);

// Configuración de Firebase (se obtiene de las variables globales del entorno Canvas)
const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');
const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';

// Inicializar Firebase
const app = initializeApp(firebaseConfig);
const db = getFirestore(app);
const auth = getAuth(app);
const storage = getStorage(app);

// Colección de pagos (pública para que el admin vea todo)
// NOTA IMPORTANTE: La ruta de la colección debe coincidir con tus reglas de seguridad de Firestore.
// Para esta aplicación, los pagos se almacenan en una colección pública para que el administrador
// pueda ver todos los pagos, mientras que los alumnos solo pueden ver los suyos.
const paymentsCollectionRef = collection(db, `artifacts/${appId}/public/data/payments`);

// Componente de Página de Inicio de Sesión (para Admin)
const LoginPage = () => {
    const { showMessage, ADMIN_EMAIL, ADMIN_PASSWORD } = useContext(AuthContext);
    const [email, setEmail] = useState('');
    const [password, setPassword] = useState('');
    const [loading, setLoading] = useState(false);

    const handleLogin = async (e) => {
        e.preventDefault();
        setLoading(true);
        try {
            if (email === ADMIN_EMAIL && password === ADMIN_PASSWORD) {
                await signInWithEmailAndPassword(auth, email, password);
                showMessage('Inicio de sesión de administrador exitoso!', 'success');
            } else {
                showMessage('Credenciales de administrador inválidas.', 'error');
            }
        } catch (error) {
            console.error("Error al iniciar sesión:", error);
            showMessage(`Error al iniciar sesión: ${error.message}`, 'error');
        } finally {
            setLoading(false);
        }
    };

    return (
        <div className="bg-white p-8 rounded-xl shadow-lg w-full max-w-md">
            <h2 className="text-2xl font-bold text-gray-800 mb-6 text-center">Acceso de Administrador</h2>
            <form onSubmit={handleLogin}>
                <div className="mb-4">
                    <label className="block text-gray-700 text-sm font-bold mb-2" htmlFor="email">
                        Email:
                    </label>
                    <input
                        type="email"
                        id="email"
                        className="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500"
                        value={email}
                        onChange={(e) => setEmail(e.target.value)}
                        required
                    />
                </div>
                <div className="mb-6">
                    <label className="block text-gray-700 text-sm font-bold mb-2" htmlFor="password">
                        Contraseña:
                    </label>
                    <input
                        type="password"
                        id="password"
                        className="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500"
                        value={password}
                        onChange={(e) => setPassword(e.target.value)}
                        required
                    />
                </div>
                <button
                    type="submit"
                    className="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-4 rounded-lg focus:outline-none focus:shadow-outline transition duration-300 ease-in-out transform hover:scale-105"
                    disabled={loading}
                >
                    {loading ? 'Ingresando...' : 'Ingresar como Admin'}
                </button>
            </form>
            <p className="text-center text-gray-600 text-sm mt-4">
                (Usa email: `admin@example.com`, contraseña: `admin123` para probar)
            </p>
        </div>
    );
};

// Componente del Panel de Administrador
const AdminDashboard = () => {
    const { user, showMessage } = useContext(AuthContext);
    const [payments, setPayments] = useState([]);
    const [loadingPayments, setLoadingPayments] = useState(true);
    const [filterName, setFilterName] = useState('');
    const [filterCourse, setFilterCourse] = useState('');
    const [filterMonth, setFilterMonth] = useState('');

    useEffect(() => {
        if (!user) return;

        setLoadingPayments(true);
        // Consulta para obtener todos los pagos (accesible para el admin)
        const q = query(paymentsCollectionRef);

        const unsubscribe = onSnapshot(q, (snapshot) => {
            const paymentsData = snapshot.docs.map(doc => ({
                id: doc.id,
                ...doc.data(),
                paymentDate: doc.data().paymentDate?.toDate ? doc.data().paymentDate.toDate().toLocaleDateString('es-AR') : doc.data().paymentDate // Convertir timestamp a fecha legible
            }));
            setPayments(paymentsData);
            setLoadingPayments(false);
        }, (error) => {
            console.error("Error al obtener pagos:", error);
            // Mostrar un mensaje más específico si es un error de permisos
            if (error.code === 'permission-denied') {
                showMessage("Error de permisos: Asegúrate de que las reglas de seguridad de Firestore permitan la lectura para usuarios autenticados en la colección 'payments'.", "error");
            } else {
                showMessage("Error al cargar los pagos.", "error");
            }
            setLoadingPayments(false);
        });

        return () => unsubscribe();
    }, [user]);

    const filteredPayments = payments.filter(payment => {
        const matchesName = payment.studentName.toLowerCase().includes(filterName.toLowerCase());
        const matchesCourse = payment.courseName.toLowerCase().includes(filterCourse.toLowerCase());
        const matchesMonth = filterMonth ? payment.paymentMonth === filterMonth : true;
        return matchesName && matchesCourse && matchesMonth;
    });

    const handleLogout = async () => {
        try {
            await signOut(auth);
            showMessage('Sesión cerrada correctamente.', 'success');
        } catch (error) {
            console.error("Error al cerrar sesión:", error);
            showMessage(`Error al cerrar sesión: ${error.message}`, 'error');
        }
    };

    const handleExportExcel = () => {
        if (filteredPayments.length === 0) {
            showMessage('No hay datos para exportar a Excel.', 'error');
            return;
        }
        // Acceder a XLSX desde el objeto global window
        const XLSX = window.XLSX;
        if (!XLSX) {
            showMessage('La librería XLSX no está cargada. Inténtalo de nuevo.', 'error');
            return;
        }

        const dataToExport = filteredPayments.map(p => ({
            'Nombre y Apellido': p.studentName,
            'Curso': p.courseName,
            'Mes de Inicio del Curso': p.startMonth,
            'Mes de la Cuota': p.paymentMonth,
            'Monto': p.amount,
            'Fecha de Pago': p.paymentDate,
            'URL Comprobante': p.proofUrl
        }));

        const ws = XLSX.utils.json_to_sheet(dataToExport);
        const wb = XLSX.utils.book_new();
        XLSX.utils.book_append_sheet(wb, ws, "Pagos");
        XLSX.writeFile(wb, "listado_pagos.xlsx");
        showMessage('Datos exportados a Excel exitosamente.', 'success');
    };

    const handleExportPdf = () => {
        if (filteredPayments.length === 0) {
            showMessage('No hay datos para exportar a PDF.', 'error');
            return;
        }
        // Acceder a jsPDF y html2canvas desde el objeto global window
        const jsPDF = window.jspdf.jsPDF;
        const html2canvas = window.html2canvas;

        if (!jsPDF || !html2canvas) {
            showMessage('Las librerías de PDF no están cargadas. Inténtalo de nuevo.', 'error');
            return;
        }

        const input = document.getElementById('paymentsTable'); // ID de la tabla para capturar

        html2canvas(input, { scale: 2 }).then(canvas => {
            const imgData = canvas.toDataURL('image/png');
            const pdf = new jsPDF('p', 'mm', 'a4');
            const imgWidth = 210; // A4 width in mm
            const pageHeight = 297; // A4 height in mm
            const imgHeight = canvas.height * imgWidth / canvas.width;
            let heightLeft = imgHeight;
            let position = 0;

            pdf.addImage(imgData, 'PNG', 0, position, imgWidth, imgHeight);
            heightLeft -= pageHeight;

            while (heightLeft >= 0) {
                position = heightLeft - imgHeight;
                pdf.addPage();
                pdf.addImage(imgData, 'PNG', 0, position, imgWidth, imgHeight);
                heightLeft -= pageHeight;
            }
            pdf.save("listado_pagos.pdf");
            showMessage('Datos exportados a PDF exitosamente.', 'success');
        }).catch(error => {
            console.error("Error al exportar a PDF:", error);
            showMessage('Error al exportar a PDF. Inténtalo de nuevo.', 'error');
        });
    };

    const months = [
        "Enero", "Febrero", "Marzo", "Abril", "Mayo", "Junio",
        "Julio", "Agosto", "Septiembre", "Octubre", "Noviembre", "Diciembre"
    ];

    return (
        <div className="bg-white p-8 rounded-xl shadow-lg w-full max-w-6xl">
            <div className="flex justify-between items-center mb-6">
                <h2 className="text-2xl font-bold text-gray-800">Panel de Administrador</h2>
                <button
                    onClick={handleLogout}
                    className="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded-lg transition duration-300 ease-in-out transform hover:scale-105"
                >
                    Cerrar Sesión
                </button>
            </div>

            <div className="mb-6 grid grid-cols-1 md:grid-cols-3 gap-4">
                <div>
                    <label htmlFor="filterName" className="block text-gray-700 text-sm font-bold mb-2">Filtrar por Nombre:</label>
                    <input
                        type="text"
                        id="filterName"
                        className="shadow appearance-none border rounded-lg w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500"
                        value={filterName}
                        onChange={(e) => setFilterName(e.target.value)}
                        placeholder="Nombre del alumno"
                    />
                </div>
                <div>
                    <label htmlFor="filterCourse" className="block text-gray-700 text-sm font-bold mb-2">Filtrar por Curso:</label>
                    <input
                        type="text"
                        id="filterCourse"
                        className="shadow appearance-none border rounded-lg w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500"
                        value={filterCourse}
                        onChange={(e) => setFilterCourse(e.target.value)}
                        placeholder="Nombre del curso"
                    />
                </div>
                <div>
                    <label htmlFor="filterMonth" className="block text-gray-700 text-sm font-bold mb-2">Filtrar por Mes de Cuota:</label>
                    <select
                        id="filterMonth"
                        className="shadow appearance-none border rounded-lg w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500"
                        value={filterMonth}
                        onChange={(e) => setFilterMonth(e.target.value)}
                    >
                        <option value="">Todos los Meses</option>
                        {months.map(month => (
                            <option key={month} value={month}>{month}</option>
                        ))}
                    </select>
                </div>
            </div>

            <div className="mb-6 flex gap-4">
                <button
                    onClick={handleExportExcel}
                    className="flex-1 bg-green-600 hover:bg-green-700 text-white font-bold py-3 px-4 rounded-lg transition duration-300 ease-in-out transform hover:scale-105"
                >
                    Descargar Excel
                </button>
                <button
                    onClick={handleExportPdf}
                    className="flex-1 bg-red-600 hover:bg-red-700 text-white font-bold py-3 px-4 rounded-lg transition duration-300 ease-in-out transform hover:scale-105"
                >
                    Descargar PDF
                </button>
            </div>

            {loadingPayments ? (
                <LoadingSpinner />
            ) : (
                <div className="overflow-x-auto">
                    <table id="paymentsTable" className="min-w-full bg-white border border-gray-200 rounded-lg">
                        <thead>
                            <tr className="bg-gray-200 text-gray-700 uppercase text-sm leading-normal">
                                <th className="py-3 px-6 text-left">Nombre Alumno</th>
                                <th className="py-3 px-6 text-left">Curso</th>
                                <th className="py-3 px-6 text-left">Mes Inicio</th>
                                <th className="py-3 px-6 text-left">Mes Cuota</th>
                                <th className="py-3 px-6 text-left">Monto</th>
                                <th className="py-3 px-6 text-left">Fecha Pago</th>
                                <th className="py-3 px-6 text-left">Comprobante</th>
                            </tr>
                        </thead>
                        <tbody className="text-gray-600 text-sm font-light">
                            {filteredPayments.length > 0 ? (
                                filteredPayments.map(payment => (
                                    <tr key={payment.id} className="border-b border-gray-200 hover:bg-gray-100">
                                        <td className="py-3 px-6 text-left whitespace-nowrap">{payment.studentName}</td>
                                        <td className="py-3 px-6 text-left">{payment.courseName}</td>
                                        <td className="py-3 px-6 text-left">{payment.startMonth}</td>
                                        <td className="py-3 px-6 text-left">{payment.paymentMonth}</td>
                                        <td className="py-3 px-6 text-left">${payment.amount}</td>
                                        <td className="py-3 px-6 text-left">{payment.paymentDate}</td>
                                        <td className="py-3 px-6 text-left">
                                            {payment.proofUrl ? (
                                                <a href={payment.proofUrl} target="_blank" rel="noopener noreferrer" className="text-blue-500 hover:underline">Ver Comprobante</a>
                                            ) : 'N/A'}
                                        </td>
                                    </tr>
                                ))
                            ) : (
                                <tr>
                                    <td colSpan="7" className="py-4 text-center text-gray-500">No hay pagos registrados o que coincidan con los filtros.</td>
                                </tr>
                            )}
                        </tbody>
                    </table>
                </div>
            )}
        </div>
    );
};

// Componente de Página de Registro de Pagos para Alumnos
const StudentPaymentPage = () => {
    const { user, showMessage } = useContext(AuthContext);
    const [studentName, setStudentName] = useState('');
    const [courseName, setCourseName] = useState('');
    const [startMonth, setStartMonth] = useState('');
    const [paymentMonth, setPaymentMonth] = useState('');
    const [amount, setAmount] = useState('');
    const [paymentDate, setPaymentDate] = useState('');
    const [proofFile, setProofFile] = useState(null);
    const [loading, setLoading] = useState(false);
    const [myPayments, setMyPayments] = useState([]);
    const [loadingMyPayments, setLoadingMyPayments] = useState(true);

    const months = [
        "Enero", "Febrero", "Marzo", "Abril", "Mayo", "Junio",
        "Julio", "Agosto", "Septiembre", "Octubre", "Noviembre", "Diciembre"
    ];

    // Cargar pagos del usuario actual
    useEffect(() => {
        if (!user) return;

        setLoadingMyPayments(true);
        // Consulta para obtener solo los pagos realizados por este usuario (basado en el UID)
        const q = query(paymentsCollectionRef, where("registeredByUserId", "==", user.uid));

        const unsubscribe = onSnapshot(q, (snapshot) => {
            const paymentsData = snapshot.docs.map(doc => ({
                id: doc.id,
                ...doc.data(),
                paymentDate: doc.data().paymentDate?.toDate ? doc.data().paymentDate.toDate().toLocaleDateString('es-AR') : doc.data().paymentDate
            }));
            setMyPayments(paymentsData);
            setLoadingMyPayments(false);
        }, (error) => {
            console.error("Error al obtener mis pagos:", error);
            // Mostrar un mensaje más específico si es un error de permisos
            if (error.code === 'permission-denied') {
                showMessage("Error de permisos: Asegúrate de que las reglas de seguridad de Firestore permitan la lectura y escritura para usuarios autenticados en la colección 'payments', y que los usuarios solo puedan leer sus propios documentos si la colección fuera privada.", "error");
            } else {
                showMessage("Error al cargar tus pagos.", "error");
            }
            setLoadingMyPayments(false);
        });

        return () => unsubscribe();
    }, [user]);

    const handleFileChange = (e) => {
        if (e.target.files[0]) {
            setProofFile(e.target.files[0]);
        }
    };

    const handleSubmitPayment = async (e) => {
        e.preventDefault();
        setLoading(true);

        if (!proofFile) {
            showMessage('Por favor, sube un comprobante de pago.', 'error');
            setLoading(false);
            return;
        }

        try {
            // 1. Subir comprobante a Firebase Storage
            const storageRef = ref(storage, `comprobantes/${user.uid}/${Date.now()}_${proofFile.name}`);
            const uploadTask = await uploadBytes(storageRef, proofFile);
            const proofUrl = await getDownloadURL(uploadTask.ref);

            // 2. Guardar datos del pago en Firestore
            await addDoc(paymentsCollectionRef, {
                studentName,
                courseName,
                startMonth,
                paymentMonth,
                amount: parseFloat(amount), // Convertir a número
                paymentDate: new Date(paymentDate), // Convertir a Timestamp de Firestore
                proofUrl,
                registeredByUserId: user.uid, // Para identificar quién registró el pago
                createdAt: new Date()
            });

            showMessage('Pago registrado con éxito!', 'success');
            // Limpiar formulario
            setStudentName('');
            setCourseName('');
            setStartMonth('');
            setPaymentMonth('');
            setAmount('');
            setPaymentDate('');
            setProofFile(null);
            document.getElementById('proofFile').value = ''; // Limpiar input file
        } catch (error) {
            console.error("Error al registrar pago:", error);
            // Mostrar un mensaje más específico si es un error de permisos
            if (error.code === 'permission-denied') {
                showMessage("Error de permisos: Asegúrate de que las reglas de seguridad de Firestore permitan la escritura para usuarios autenticados en la colección 'payments'.", "error");
            } else {
                showMessage(`Error al registrar pago: ${error.message}`, 'error');
            }
        } finally {
            setLoading(false);
        }
    };

    const handleLogout = async () => {
        try {
            await signOut(auth);
            showMessage('Sesión cerrada correctamente.', 'success');
        } catch (error) {
            console.error("Error al cerrar sesión:", error);
            showMessage(`Error al cerrar sesión: ${error.message}`, 'error');
        }
    };

    return (
        <div className="bg-white p-8 rounded-xl shadow-lg w-full max-w-4xl">
            <div className="flex justify-between items-center mb-6">
                <h2 className="text-2xl font-bold text-gray-800">Panel de Registro de Pagos</h2>
                <button
                    onClick={handleLogout}
                    className="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded-lg transition duration-300 ease-in-out transform hover:scale-105"
                >
                    Cerrar Sesión
                </button>
            </div>

            <form onSubmit={handleSubmitPayment} className="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
                <div>
                    <label className="block text-gray-700 text-sm font-bold mb-2" htmlFor="studentName">
                        Nombre y Apellido:
                    </label>
                    <input
                        type="text"
                        id="studentName"
                        className="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500"
                        value={studentName}
                        onChange={(e) => setStudentName(e.target.value)}
                        required
                    />
                </div>
                <div>
                    <label className="block text-gray-700 text-sm font-bold mb-2" htmlFor="courseName">
                        Curso:
                    </label>
                    <input
                        type="text"
                        id="courseName"
                        className="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500"
                        value={courseName}
                        onChange={(e) => setCourseName(e.target.value)}
                        required
                    />
                </div>
                <div>
                    <label className="block text-gray-700 text-sm font-bold mb-2" htmlFor="startMonth">
                        Mes de Inicio del Curso:
                    </label>
                    <select
                        id="startMonth"
                        className="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500"
                        value={startMonth}
                        onChange={(e) => setStartMonth(e.target.value)}
                        required
                    >
                        <option value="">Selecciona un mes</option>
                        {months.map(month => (
                            <option key={month} value={month}>{month}</option>
                        ))}
                    </select>
                </div>
                <div>
                    <label className="block text-gray-700 text-sm font-bold mb-2" htmlFor="paymentMonth">
                        Mes de la Cuota (12 cuotas):
                    </label>
                    <select
                        id="paymentMonth"
                        className="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500"
                        value={paymentMonth}
                        onChange={(e) => setPaymentMonth(e.target.value)}
                        required
                    >
                        <option value="">Selecciona un mes</option>
                        {months.map(month => (
                            <option key={month} value={month}>{month}</option>
                        ))}
                    </select>
                </div>
                <div>
                    <label className="block text-gray-700 text-sm font-bold mb-2" htmlFor="amount">
                        Monto:
                    </label>
                    <input
                        type="number"
                        id="amount"
                        className="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500"
                        value={amount}
                        onChange={(e) => setAmount(e.target.value)}
                        required
                        step="0.01"
                    />
                </div>
                <div>
                    <label className="block text-gray-700 text-sm font-bold mb-2" htmlFor="paymentDate">
                        Fecha de Pago:
                    </label>
                    <input
                        type="date"
                        id="paymentDate"
                        className="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500"
                        value={paymentDate}
                        onChange={(e) => setPaymentDate(e.target.value)}
                        required
                    />
                </div>
                <div className="md:col-span-2">
                    <label className="block text-gray-700 text-sm font-bold mb-2" htmlFor="proofFile">
                        Comprobante de Pago (Imagen/PDF):
                    </label>
                    <input
                        type="file"
                        id="proofFile"
                        className="shadow appearance-none border rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500"
                        onChange={handleFileChange}
                        accept="image/*,.pdf"
                        required
                    />
                </div>
                <div className="md:col-span-2">
                    <button
                        type="submit"
                        className="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-4 rounded-lg focus:outline-none focus:shadow-outline transition duration-300 ease-in-out transform hover:scale-105"
                        disabled={loading}
                    >
                        {loading ? 'Registrando Pago...' : 'Registrar Pago'}
                    </button>
                </div>
            </form>

            <h3 className="text-xl font-bold text-gray-800 mb-4">Mis Pagos Registrados</h3>
            {loadingMyPayments ? (
                <LoadingSpinner />
            ) : (
                <div className="overflow-x-auto">
                    <table className="min-w-full bg-white border border-gray-200 rounded-lg">
                        <thead>
                            <tr className="bg-gray-200 text-gray-700 uppercase text-sm leading-normal">
                                <th className="py-3 px-6 text-left">Curso</th>
                                <th className="py-3 px-6 text-left">Mes Cuota</th>
                                <th className="py-3 px-6 text-left">Monto</th>
                                <th className="py-3 px-6 text-left">Fecha Pago</th>
                                <th className="py-3 px-6 text-left">Comprobante</th>
                            </tr>
                        </thead>
                        <tbody className="text-gray-600 text-sm font-light">
                            {myPayments.length > 0 ? (
                                myPayments.map(payment => (
                                    <tr key={payment.id} className="border-b border-gray-200 hover:bg-gray-100">
                                        <td className="py-3 px-6 text-left whitespace-nowrap">{payment.courseName}</td>
                                        <td className="py-3 px-6 text-left">{payment.paymentMonth}</td>
                                        <td className="py-3 px-6 text-left">${payment.amount}</td>
                                        <td className="py-3 px-6 text-left">{payment.paymentDate}</td>
                                        <td className="py-3 px-6 text-left">
                                            {payment.proofUrl ? (
                                                <a href={payment.proofUrl} target="_blank" rel="noopener noreferrer" className="text-blue-500 hover:underline">Ver Comprobante</a>
                                            ) : 'N/A'}
                                        </td>
                                    </tr>
                                ))
                            ) : (
                                <tr>
                                    <td colSpan="5" className="py-4 text-center text-gray-500">Aún no has registrado ningún pago.</td>
                                </tr>
                            )}
                        </tbody>
                    </table>
                </div>
            )}
        </div>
    );
};

// Componente principal de la aplicación
const App = () => {
    const [user, setUser] = useState(null);
    const [loading, setLoading] = useState(true);
    const [message, setMessage] = useState('');
    const [messageType, setMessageType] = useState('error');

    // Admin de ejemplo (para la demostración, en un entorno real NO hardcodear)
    const ADMIN_EMAIL = 'admin@example.com';
    const ADMIN_PASSWORD = 'admin123';

    useEffect(() => {
        // Cargar las librerías de exportación desde CDN
        const loadScript = (src, id) => {
            if (!document.getElementById(id)) {
                const script = document.createElement('script');
                script.src = src;
                script.id = id;
                script.async = true;
                document.body.appendChild(script);
            }
        };

        loadScript('https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.17.0/xlsx.full.min.js', 'xlsx-script');
        loadScript('https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js', 'jspdf-script');
        loadScript('https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js', 'html2canvas-script');

        const unsubscribe = onAuthStateChanged(auth, async (currentUser) => {
            if (currentUser) {
                setUser(currentUser);
            } else {
                // Si no hay usuario, intentar iniciar sesión anónimamente
                try {
                    await signInAnonymously(auth);
                } catch (error) {
                    console.error("Error al iniciar sesión anónimamente:", error);
                    showMessage("Error al iniciar sesión anónimamente.", "error");
                }
            }
            setLoading(false);
        });
        return () => unsubscribe();
    }, []);

    const showMessage = (msg, type) => {
        setMessage(msg);
        setMessageType(type);
        setTimeout(() => {
            setMessage('');
        }, 5000); // El mensaje desaparece después de 5 segundos
    };

    const isAdmin = user && user.email === ADMIN_EMAIL;

    if (loading) {
        return <LoadingSpinner />;
    }

    return (
        <AuthContext.Provider value={{ user, isAdmin, showMessage, ADMIN_EMAIL, ADMIN_PASSWORD }}>
            <div className="min-h-screen bg-gray-100 flex flex-col items-center justify-center p-4">
                <MessageBox message={message} type={messageType} onClose={() => setMessage('')} />
                <h1 className="text-4xl font-extrabold text-blue-700 mb-8 text-center">
                    Sistema de Registro de Pagos
                </h1>

                {user ? (
                    isAdmin ? (
                        <AdminDashboard />
                    ) : (
                        <StudentPaymentPage />
                    )
                ) : (
                    <LoginPage />
                )}
            </div>
        </AuthContext.Provider>
    );
};

export default App;
