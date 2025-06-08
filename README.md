import React, { useState, useRef, useEffect } from "react";

// Cambia estos nombres por los que necesitas y asegúrate de que coincidan con tus carpetas de imágenes
const categories = [
  "FILTRO DE GASOLINA EXTERIOR",
  "FILTRO DE GASOLINA SUMERGIBLE",
  "FILTRO DE AIRE",
  "FILTRO DE AIRE ACONDICIONADO",
  "PRE FILTROS",
  "EMPAQUE DE TAPA DE BALANCINES",
  "FILTRO DE PETROLEO",
  "REGULADOR DE PRESION",
  "KIT DE DISTRIBUCION",
  "KIT DE EMBRAGUE",
  "SOPORTE DE MOTOR",
  "VALVULA IAC",
  "SENSOR DE TEMPERATURA DE REFRIGERANTE",
  "BOBINA DE ENCENDIDO"
];

const products = [
  {
    id: 1,
    name: "Filtro de Gasolina Exterior",
    category: "FILTRO DE GASOLINA EXTERIOR",
    images: Array.from({ length: 5 }, (_, i) => ({
      src: `/images/filtros/gasolina-exterior-${i + 1}.jpg`,
      alt: `Filtro Gasolina Exterior ${i + 1}`,
    })),
  },
  {
    id: 2,
    name: "Filtro de Gasolina Sumergible",
    category: "FILTRO DE GASOLINA SUMERGIBLE",
    images: Array.from({ length: 5 }, (_, i) => ({
      src: `/images/filtros/gasolina-sumerjible-${i + 1}.jpg`,
      alt: `Filtro Sumergible ${i + 1}`,
    })),
  },
  {
    id: 3,
    name: "Filtro de Aire Estándar",
    category: "FILTRO DE AIRE",
    images: Array.from({ length: 7 }, (_, i) => ({
      src: `/images/filtros/aire-${i + 1}.jpg`,
      alt: `Filtro Aire ${i + 1}`,
    })),
  },
  {
    id: 4,
    name: "Filtro de Aire Acondicionado",
    category: "FILTRO DE AIRE ACONDICIONADO",
    images: Array.from({ length: 6 }, (_, i) => ({
      src: `/images/filtros/ac-${i + 1}.jpg`,
      alt: `Filtro AC ${i + 1}`,
    })),
  },
  {
    id: 5,
    name: "Prefiltro de Combustible",
    category: "PRE FILTROS",
    images: Array.from({ length: 8 }, (_, i) => ({
      src: `/images/filtros/prefiltro-${i + 1}.jpg`,
      alt: `Prefiltro ${i + 1}`,
    })),
  },
  {
    id: 6,
    name: "Empaque Tapa de Balancines",
    category: "EMPAQUE DE TAPA DE BALANCINES",
    images: Array.from({ length: 4 }, (_, i) => ({
      src: `/images/componentes/empaque-balancin-${i + 1}.jpg`,
      alt: `Empaque Balancines ${i + 1}`,
    })),
  },
  {
    id: 7,
    name: "Filtro de Petróleo Premium",
    category: "FILTRO DE PETROLEO",
    images: Array.from({ length: 10 }, (_, i) => ({
      src: `/images/filtros/petroleo-${i + 1}.jpg`,
      alt: `Filtro Petróleo ${i + 1}`,
    })),
  },
  {
    id: 8,
    name: "Regulador de Presión",
    category: "REGULADOR DE PRESION",
    images: Array.from({ length: 5 }, (_, i) => ({
      src: `/images/componentes/regulador-presion-${i + 1}.jpg`,
      alt: `Regulador Presión ${i + 1}`,
    })),
  },
  {
    id: 9,
    name: "Kit de Distribución Completo",
    category: "KIT DE DISTRIBUCION",
    images: Array.from({ length: 6 }, (_, i) => ({
      src: `/images/componentes/kit-distribucion-${i + 1}.jpg`,
      alt: `Kit Distribución ${i + 1}`,
    })),
  },
  {
    id: 10,
    name: "Kit de Embrague HD",
    category: "KIT DE EMBRAGUE",
    images: Array.from({ length: 7 }, (_, i) => ({
      src: `/images/componentes/kit-embrague-${i + 1}.jpg`,
      alt: `Kit Embrague ${i + 1}`,
    })),
  },
  {
    id: 11,
    name: "Soporte Motor Reforzado",
    category: "SOPORTE DE MOTOR",
    images: Array.from({ length: 5 }, (_, i) => ({
      src: `/images/componentes/soporte-motor-${i + 1}.jpg`,
      alt: `Soporte Motor ${i + 1}`,
    })),
  },
  {
    id: 12,
    name: "Válvula IAC Automotriz",
    category: "VALVULA IAC",
    images: Array.from({ length: 6 }, (_, i) => ({
      src: `/images/componentes/valvula-iac-${i + 1}.jpg`,
      alt: `Valvula IAC ${i + 1}`,
    })),
  },
  {
    id: 13,
    name: "Sensor Temperatura Refrigerante",
    category: "SENSOR DE TEMPERATURA DE REFRIGERANTE",
    images: Array.from({ length: 5 }, (_, i) => ({
      src: `/images/componentes/sensor-temp-${i + 1}.jpg`,
      alt: `Sensor Temp Refrigerante ${i + 1}`,
    })),
  },
  {
    id: 14,
    name: "Bobina de Encendido Universal",
    category: "BOBINA DE ENCENDIDO",
    images: Array.from({ length: 10 }, (_, i) => ({
      src: `/images/bobinas/bobina-${i + 1}.jpg`,
      alt: `Bobina de Encendido ${i + 1}`,
    })),
  },
];

export default function App() {
  const [selectedCategory, setSelectedCategory] = useState(null);
  const [searchTerm, setSearchTerm] = useState("");
  const [modalOpen, setModalOpen] = useState(false);
  const [selectedImageIndex, setSelectedImageIndex] = useState(0);
  const [selectedProduct, setSelectedProduct] = useState(null);
  const modalRef = useRef(null);

  // Filtrado
  const filteredProducts = products.filter((product) => {
    const matchesCategory = selectedCategory ? product.category === selectedCategory : true;
    const matchesSearch = product.name.toLowerCase().includes(searchTerm.toLowerCase());
    return matchesCategory && matchesSearch;
  });

  // Modal: abrir/cerrar
  const openModal = (product, index = 0) => {
    setSelectedImageIndex(index);
    setSelectedProduct(product);
    setModalOpen(true);
  };
  const closeModal = () => setModalOpen(false);

  // Navegación galería
  const nextImage = () => {
    if (selectedProduct && selectedImageIndex < selectedProduct.images.length - 1) {
      setSelectedImageIndex(selectedImageIndex + 1);
    }
  };
  const prevImage = () => {
    if (selectedImageIndex > 0) setSelectedImageIndex(selectedImageIndex - 1);
  };

  // Cerrar modal con ESC o click afuera
  useEffect(() => {
    if (!modalOpen) return;
    const handleKey = (e) => {
      if (e.key === "Escape") closeModal();
      if (e.key === "ArrowRight") nextImage();
      if (e.key === "ArrowLeft") prevImage();
    };
    window.addEventListener("keydown", handleKey);
    return () => window.removeEventListener("keydown", handleKey);
  }, [modalOpen, selectedImageIndex, selectedProduct]);

  // Click fuera del modal
  const handleOverlayClick = (e) => {
    if (modalRef.current && e.target === modalRef.current) closeModal();
  };

  return (
    <div className="min-h-screen bg-gray-50">
      {/* Header */}
      <header className="bg-blue-600 text-white shadow-md">
        <div className="container mx-auto px-4 py-4 flex justify-between items-center">
          <h1 className="text-2xl font-bold">Catálogo Repuestos Automotrices</h1>
          <input
            type="text"
            placeholder="Buscar producto..."
            value={searchTerm}
            onChange={(e) => setSearchTerm(e.target.value)}
            className="w-64 p-2 rounded-md text-gray-800 focus:outline-none"
          />
        </div>
      </header>

      {/* Categorías */}
      <main className="container mx-auto px-4 py-8">
        <div className="flex flex-wrap gap-2 mb-6 justify-center">
          <button
            onClick={() => setSelectedCategory(null)}
            className={`px-4 py-2 rounded-full transition-colors ${
              selectedCategory === null
                ? "bg-blue-600 text-white"
                : "bg-gray-200 text-gray-800 hover:bg-gray-300"
            }`}
          >
            Todos
          </button>
          {categories.map((category) => (
            <button
              key={category}
              onClick={() => setSelectedCategory(category)}
              className={`px-4 py-2 rounded-full transition-colors whitespace-nowrap ${
                selectedCategory === category
                  ? "bg-blue-600 text-white"
                  : "bg-gray-200 text-gray-800 hover:bg-gray-300"
              }`}
            >
              {category}
            </button>
          ))}
        </div>

        {/* Productos */}
        <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
          {filteredProducts.length > 0 ? (
            filteredProducts.map((product) => (
              <div
                key={product.id}
                className="bg-white rounded-lg overflow-hidden shadow-md hover:shadow-xl transition-shadow duration-300 cursor-pointer"
                onClick={() => openModal(product)}
                tabIndex={0}
              >
                <img
                  src={product.images[0].src}
                  alt={product.images[0].alt}
                  className="w-full h-48 object-cover"
                />
                <div className="p-4">
                  <h3 className="font-semibold text-lg mb-2">{product.name}</h3>
                  <p className="text-sm text-gray-600">{product.category}</p>
                </div>
              </div>
            ))
          ) : (
            <div className="col-span-full text-center py-12">
              <p className="text-gray-500 text-lg">
                No se encontraron productos con los criterios seleccionados.
              </p>
            </div>
          )}
        </div>
      </main>

      {/* Modal */}
      {modalOpen && selectedProduct && (
        <div
          className="fixed inset-0 z-50 flex items-center justify-center bg-black bg-opacity-70"
          ref={modalRef}
          onClick={handleOverlayClick}
        >
          <div className="bg-white rounded-lg max-w-4xl w-full max-h-[90vh] overflow-y-auto relative animate-fadeIn">
            <button
              onClick={closeModal}
              className="absolute top-4 right-4 text-gray-600 hover:text-gray-900 text-3xl font-bold"
              aria-label="Cerrar galería"
            >
              &times;
            </button>
            <div className="flex flex-col items-center">
              <img
                src={selectedProduct.images[selectedImageIndex].src}
                alt={selectedProduct.images[selectedImageIndex].alt}
                className="max-w-full max-h-[60vh] object-contain rounded shadow"
              />
              {/* Miniaturas */}
              <div className="flex gap-2 mt-4 pb-2 flex-wrap justify-center">
                {selectedProduct.images.map((img, i) => (
                  <img
                    key={img.src}
                    src={img.src}
                    alt={img.alt}
                    className={`w-16 h-16 object-cover rounded border-2 ${
                      selectedImageIndex === i
                        ? "border-blue-600"
                        : "border-gray-300"
                    } cursor-pointer`}
                    onClick={() => setSelectedImageIndex(i)}
                  />
                ))}
              </div>
              <div className="flex justify-between w-full mt-2 px-4 pb-4">
                <button
                  onClick={prevImage}
                  disabled={selectedImageIndex === 0}
                  className={`px-4 py-2 rounded ${
                    selectedImageIndex === 0
                      ? "opacity-50 cursor-not-allowed"
                      : "bg-blue-600 text-white"
                  }`}
                >
                  Anterior
                </button>
                <span className="self-center bg-gray-200 text-gray-900 px-3 py-1 rounded shadow">
                  {selectedImageIndex + 1} / {selectedProduct.images.length}
                </span>
                <button
                  onClick={nextImage}
                  disabled={selectedImageIndex === selectedProduct.images.length - 1}
                  className={`px-4 py-2 rounded ${
                    selectedImageIndex === selectedProduct.images.length - 1
                      ? "opacity-50 cursor-not-allowed"
                      : "bg-blue-600 text-white"
                  }`}
                >
                  Siguiente
                </button>
              </div>
            </div>
          </div>
        </div>
      )}

      {/* Footer */}
      <footer className="bg-gray-800 text-white py-6 mt-12">
        <div className="container mx-auto px-4 text-center">
          <p>
            &copy; 2025 Catálogo Repuestos Automotrices. Todos los derechos reservados.
          </p>
        </div>
      </footer>
    </div>
  );
}

// Si usas Tailwind, puedes agregar la animación opcionalmente en tu CSS:
// .animate-fadeIn { animation: fadeIn 0.3s; }
// @keyframes fadeIn { from { opacity: 0; transform: scale(0.98);} to { opacity: 1; transform: scale(1);} }
