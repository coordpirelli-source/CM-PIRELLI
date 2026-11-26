const AI27_RUTAS_CORREDORES = Object.freeze([
  ['LOGISTICA INVER', 'LOGISTICA INVER', 'SILAO', 'LAREDO', 876],
  ['NORT-1', 'NORTE 1', 'SILAO', 'SAN LUIS', 167],
  ['NORT-2', 'NORTE 2', 'SAN LUIS', 'MATEHUALA', 189],
  ['NORT-3', 'NORTE 3', 'MATEHUALA', 'PARADOR SAN PEDRO', 122],
  ['NORT-4', 'NORTE 4', 'PARADOR SAN PEDRO', 'CASETA DE LINCON', 198],
  ['NORT-5', 'NORTE 5', 'PERIFERICO DE MTY', 'CAETA DE SABINAS', 103],
  ['PACIF-1', 'PACFC-SUR 1', 'SILAO', 'PALMILLAS BIS', 198],
  ['PACIF-2', '', 'PALMILLAS BIS', 'CASETA DE SANTORUM', 207],
  ['SUR 2', 'SURESTE 2', 'CASETA DE SANTORUM', 'PUEBLA', 123],
  ['BAJIO', 'BAJIO 1', 'SILAO', 'CASETE DE CERRITOS', 327],
  ['NOREST-1', 'NORESTE 1', 'CASETE DE CERRITOS', 'ENTRONQUE DE ESTACION MANUEL', 127],
  ['NOREST-2', 'NORESTE 2', 'ENTRONQUE DE ESTACION MANUEL', 'ALTAMIRA', 220],
  ['RECOL- NVL', 'RECOLECTOR LAREDO', 'LA GARITA', 'NUEVO LAREDO', 41],
  ['RECOL-MAZ', 'RECOLECTOR MAZANILLO', 'COLIAMA', 'MANZANILLO', 107],
  ['RECOLEC- VER', 'RECOLECCION VERACRUZ', 'LAS VIGAS', 'VERACRUZ', 153],
  ['M-RECOL-SLP', 'RECOLECTOR SAN LUIS METRO', 'CESETA DE CALDERON', 'SAN LUIS POTOSI', 37],
  ['B-RECOL-SLP', 'RECOLECTOR SAN LUIS PATRULLA', 'CESETA DE CALDERON', 'SAN LUIS POTOSI', 37]
].map(corredor => Object.freeze({
  id: corredor[0],
  nombre: corredor[1],
  origen: corredor[2],
  destino: corredor[3],
  distanciaKm: corredor[4]
})));

const AI27 = Object.freeze({
  VERSION: '3.0.0',
  SPREADSHEET_ID_SUPERVISOR: '1WW6BdSj0FhXqYWrhRX-XXLop4W5iCjTEHwVqPeWIELI',
  SPREADSHEET_ID_MONITORISTA: '1QPFVCRYfNw-QzHwVYHPE1EeFERpswQ1jCXLJsR9HYmk',
  ZONA: 'America/Mexico_City',
  FILAS: 1000,
  CORTE_INICIO: [19, 15],
  CORTE_FIN: [7, 15],
  ESTACIONES: ['ESTACION 1', 'ESTACION 2', 'ESTACION 3'],
  CORREDORES: AI27_RUTAS_CORREDORES.map(corredor => corredor.nombre || corredor.id),
  SERVICIOS: [
    'Equipo Original', 'Pirelli PPE', 'Pirelli Distribución',
    'Exportación aérea', 'Exportación marítima', 'Importación',
    'Exportación NL', 'Exportación SLP', 'Exportación Tuxpan'
  ],
  SUBTIPOS: [
    'Pago por evento inteligente con custodia', 'Equipo Original',
    'Distribución', 'Exportación aérea', 'Exportación marítima',
    'Importación', 'Exportación Nuevo Laredo', 'Exportación San Luis',
    'Exportación Tuxpan'
  ],
  VALIDADORES: [
    'Autoridades', 'Grupo de reacción', 'Corredor dedicado', 'Validación remota'
  ],
  ESTADOS_VALIDACION: ['Pendiente', 'Validada'],
  ESTATUS: [
    'Pendiente', 'Asignado', 'En recorrido', 'En ruta',
    'En parada autorizada', 'Detenida', 'En destino',
    'Cierre de servicio', 'Finalizado', 'Cancelado'
  ],
  INCIDENCIAS: [
    'Sin incidencia', 'Desvío de ruta', 'Botón de pánico',
    'Operador/custodio no responde', 'Robo en progreso', 'Falla mecánica'
  ],
  INCIDENCIAS_CRITICAS: [
    'Desvío de ruta', 'Botón de pánico',
    'Operador/custodio no responde', 'Robo en progreso', 'Falla mecánica'
  ],
  COLOR: '#17365D',
  COLOR_TEXTO: '#FFFFFF'
});

const AI27_HOJAS_SUPERVISOR = [
  {
    nombre: 'CARGA DE BITÁCORAS',
    encabezados: ['Campo', 'Valor', 'Instrucciones'],
    filas: [
      ['Archivo de origen', '', 'Nombre de referencia'],
      ['Enlaces de fichas AI27 o Google Sheets', '', 'Pegar una URL por línea. Se extraerán solo los campos requeridos.'],
      ['Modo de carga', 'Individual', 'Selecciona Masivo para procesar ambas ligas.'],
      ['Iniciar carga', 'NO', 'Cambiar a SI para procesar'],
      ['Resultado', 'Pendiente', 'Resultado de la carga']
    ],
    anchos: [190, 420, 450]
  },
  {
    nombre: 'Base de datos',
    encabezados: [
      'Fecha registro', 'Hora registro', 'Cliente', 'Bitácora',
      'Servicio', 'Subtipo', 'Tipo de monitoreo', 'Línea', 'Eco',
      'Placas', 'Operador', 'Teléfono', 'Origen', 'Destino',
      'Corredor', 'Estación', 'Estatus'
    ],
    anchos: [130, 110, 160, 150, 190, 210, 180, 160, 90, 110, 180, 130, 180, 180, 170, 130, 150]
  },
  {
    nombre: 'Monitoreo En tiempo real',
    encabezados: [
      'Estación', 'Cliente', 'Servicio', 'Subtipo', 'Tipo de monitoreo',
      'Destino', 'Placas', 'Bitácora', 'Línea', 'Eco', 'Operador',
      'Último reporte', 'Estatus', 'Incidencia', 'Corredor',
      'Fecha actualización', 'Publicado'
    ],
    anchos: [130, 160, 190, 210, 180, 180, 110, 150, 160, 90, 180, 220, 170, 220, 170, 160, 110]
  },
  {
    nombre: 'Validaciones',
    encabezados: [
      'Fecha inicio', 'Bitácora', 'Cliente', 'Servicio', 'Estación',
      'Incidencia', 'Último reporte', 'Validado por', 'Fecha validación',
      'Minutos transcurridos', 'Estado', 'Observaciones'
    ],
    anchos: [160, 150, 160, 190, 130, 220, 260, 180, 160, 180, 150, 280]
  },
  {
    nombre: 'Cierres',
    encabezados: [
      'Fecha en destino', 'Bitácora', 'Cliente', 'Servicio', 'Estación',
      'Estado cierre', 'Fecha aprobación', 'Cerrado por', 'Observaciones'
    ],
    anchos: [160, 150, 160, 190, 130, 180, 160, 180, 280]
  },
  {
    nombre: 'Corredores',
    encabezados: [
      'ID_Corredor', 'Nombre_Corredor', 'Origen_Base',
      'Destino_Final', 'Distancia_Total_Km'
    ],
    filas: AI27_RUTAS_CORREDORES.map(corredor => [
      corredor.id,
      corredor.nombre,
      corredor.origen,
      corredor.destino,
      corredor.distanciaKm
    ]),
    anchos: [150, 250, 220, 260, 180]
  },
  {
    nombre: '_Cerradas',
    encabezados: [
      'Fecha en destino', 'Bitácora', 'Cliente', 'Servicio', 'Estación',
      'Estado cierre', 'Fecha aprobación', 'Cerrado por', 'Observaciones'
    ],
    anchos: [160, 150, 160, 190, 130, 180, 160, 180, 280]
  },
  {
    nombre: '_Errores',
    encabezados: ['Fecha y hora', 'Módulo', 'Función', 'Mensaje', 'Detalle'],
    anchos: [170, 150, 180, 300, 400]
  },
  {
    nombre: 'Catalogo',
    encabezados: [
      'Estaciones', 'Estatus', 'Incidencias', 'Modo de carga',
      'Corredores', 'Servicios', 'Subtipos', 'Validadores'
    ],
    anchos: [180, 210, 230, 160, 210, 240, 260, 210]
  },
  {
    nombre: 'Dashboard_Datos',
    encabezados: ['Fecha', 'Estación', 'Servicios', 'En ruta', 'Detenidos', 'Finalizados'],
    anchos: [130, 130, 150, 110, 110, 130]
  }
];

const AI27_HOJAS_MONITORISTA = [
  ...AI27.ESTACIONES.map(nombre => ({
    nombre,
    encabezados: [
      'Servicio', 'Cliente', 'Subtipo', 'Tipo de monitoreo', 'Destino',
      'Placas', 'Bitácora', 'Línea', 'Eco', 'Operador', 'Último reporte',
      'Estatus', 'Incidencia', 'Corredor', 'Actualización'
    ],
    anchos: [190, 160, 210, 180, 180, 110, 150, 160, 90, 180, 260, 170, 220, 170, 160]
  })),
  {
    nombre: 'Validaciones',
    encabezados: [
      'Fecha inicio', 'Bitácora', 'Cliente', 'Servicio', 'Estación',
      'Incidencia', 'Último reporte', 'Validado por', 'Fecha validación',
      'Minutos transcurridos', 'Estado', 'Observaciones'
    ],
    anchos: [160, 150, 160, 190, 130, 220, 260, 180, 160, 180, 150, 280]
  },
  {
    nombre: 'Catalogo',
    encabezados: [
      'Estaciones', 'Estatus', 'Incidencias', 'Corredores',
      'Servicios', 'Subtipos', 'Validadores'
    ],
    anchos: [180, 210, 230, 210, 240, 260, 210]
  }
];

function onOpen() {
  SpreadsheetApp.getUi()
    .createMenu('AI27 PIRELLI 2.0')
    .addItem('Crear estructura inicial', 'AI27_crearEstructuraInicial')
    .addItem('Verificar estructura', 'AI27_verificarEstructura')
    .addItem('Aplicar listas desplegables', 'AI27_aplicarValidaciones')
    .addItem('Mostrar turno actual', 'AI27_mostrarTurnoActual')
    .addItem('Instalar automatizaciones', 'AI27_instalarDisparadores')
    .addSeparator()
    .addItem('Procesar carga (fichas y hojas)', 'AI27_procesarCarga')
    .addItem('Enviar seleccionadas a Monitoreo', 'AI27_enviarSeleccionadasMonitoreo')
    .addItem('Revisar transferencias', 'AI27_revisarTransferencias')
    .addItem('Publicar seleccionadas a estaciones', 'AI27_publicarSeleccionadas')
    .addItem('Sincronizar avances', 'AI27_sincronizarAvances')
    .addItem('Aprobar cierre seleccionado', 'AI27_cerrarSeleccionado')
    .addItem('Resolver incidencia seleccionada', 'AI27_resolverIncidenciaSeleccionada')
    .addSeparator()
    .addItem('Actualizar Dashboard', 'AI27_actualizarDashboard')
    .addToUi();
}

function AI27_crearEstructuraInicial() {
  const ui = SpreadsheetApp.getUi();
  const actual = SpreadsheetApp.getActiveSpreadsheet();

  if (actual.getId() !== AI27.SPREADSHEET_ID_SUPERVISOR) {
    ui.alert('Abre el libro SUPERVISOR configurado en AI27.');
    return;
  }

  const respuesta = ui.prompt(
    'Instalación AI27 PIRELLI 2.0',
    'Se actualizarán las estructuras de ambos libros y se conservarán los datos cuyas columnas coincidan. ' +
      'URL e ID de registro se retirarán. Respalda ambos libros y escribe exactamente: MIGRAR AI27',
    ui.ButtonSet.OK_CANCEL
  );

  if (respuesta.getSelectedButton() !== ui.Button.OK ||
      respuesta.getResponseText().trim() !== 'MIGRAR AI27') {
    ui.alert('Instalación cancelada.');
    return;
  }

  const lock = LockService.getScriptLock();
  lock.waitLock(30000);

  try {
    if (AI27.SPREADSHEET_ID_SUPERVISOR === AI27.SPREADSHEET_ID_MONITORISTA) {
      throw new Error('Los IDs de Supervisor y Monitorista deben ser distintos.');
    }

    const supervisor = SpreadsheetApp.openById(AI27.SPREADSHEET_ID_SUPERVISOR);
    const monitorista = SpreadsheetApp.openById(AI27.SPREADSHEET_ID_MONITORISTA);

    AI27_reconstruirLibro_(supervisor, AI27_HOJAS_SUPERVISOR, 'SUPERVISOR');
    AI27_reconstruirLibro_(monitorista, AI27_HOJAS_MONITORISTA, 'MONITORISTA');
    AI27_cargarCatalogos_(supervisor);
    AI27_cargarCatalogos_(monitorista);
    AI27_aplicarValidacionesLibro_(supervisor);
    AI27_aplicarValidacionesLibro_(monitorista);

    supervisor.setSpreadsheetTimeZone(AI27.ZONA);
    monitorista.setSpreadsheetTimeZone(AI27.ZONA);

    PropertiesService.getScriptProperties().setProperties({
      AI27_VERSION: AI27.VERSION,
      AI27_INSTALADO: new Date().toISOString()
    });

    ui.alert('Instalación terminada. Se prepararon Supervisor y Monitorista.');
  } catch (error) {
    AI27_error_('INSTALADOR', error);
    ui.alert('Error al instalar: ' + error.message);
    throw error;
  } finally {
    lock.releaseLock();
  }
}

function AI27_reconstruirLibro_(ss, definiciones, rol) {
  definiciones.forEach(def => {
    let sh = ss.getSheetByName(def.nombre);
    if (!sh) sh = ss.insertSheet(def.nombre);

    const oldLastRow = sh.getLastRow();
    const oldLastColumn = sh.getLastColumn();
    const oldHeaders = oldLastColumn
      ? sh.getRange(1, 1, 1, oldLastColumn).getDisplayValues()[0]
      : [];
    const oldRows = oldLastRow > 1 && oldLastColumn
      ? sh.getRange(2, 1, oldLastRow - 1, oldLastColumn).getValues()
      : [];
    const normalizar = valor => String(valor || '')
      .replace(/\u00a0/g, ' ')
      .normalize('NFD')
      .replace(/[\u0300-\u036f]/g, '')
      .trim()
      .toLowerCase();
    const oldIndex = {};
    oldHeaders.forEach((header, index) => {
      const key = normalizar(header);
      if (key) oldIndex[key] = index;
    });
    const hadOldService = Object.prototype.hasOwnProperty.call(oldIndex, normalizar('Servicio'));
    const hadOldMonitoringType = Object.prototype.hasOwnProperty.call(oldIndex, normalizar('Tipo de monitoreo'));
    const preservedRows = def.nombre === 'Corredores' ? [] : oldRows.map(row => def.encabezados.map(header => {
      const key = normalizar(header);
      if (key === normalizar('Servicio') && hadOldService && !hadOldMonitoringType) return '';
      if (key === normalizar('Tipo de monitoreo') && !hadOldMonitoringType && hadOldService) {
        return row[oldIndex[normalizar('Servicio')]];
      }
      if (key === normalizar('Fecha en destino') && oldIndex[normalizar('Fecha cierre')] !== undefined) {
        return row[oldIndex[normalizar('Fecha cierre')]];
      }
      if (key === normalizar('Estado cierre') && oldIndex[normalizar('Estatus final')] !== undefined) {
        return row[oldIndex[normalizar('Estatus final')]];
      }
      if (key === normalizar('Subtipo') && oldIndex[normalizar('Semáforo')] !== undefined) {
        return row[oldIndex[normalizar('Semáforo')]];
      }
      if (key === normalizar('Último reporte') && oldIndex[normalizar('Reporte')] !== undefined) {
        return row[oldIndex[normalizar('Reporte')]];
      }
      const index = oldIndex[key];
      return index === undefined ? '' : row[index];
    }));

    if (sh.getFilter()) sh.getFilter().remove();
    sh.getRange(1, 1, sh.getMaxRows(), sh.getMaxColumns())
      .getMergedRanges()
      .forEach(rango => rango.breakApart());
    sh.clear();

    const columnas = def.encabezados.length;
    if (sh.getMaxColumns() < columnas) {
      sh.insertColumnsAfter(sh.getMaxColumns(), columnas - sh.getMaxColumns());
    }
    if (sh.getMaxRows() < AI27.FILAS) {
      sh.insertRowsAfter(sh.getMaxRows(), AI27.FILAS - sh.getMaxRows());
    }

    sh.getRange(1, 1, 1, columnas)
      .setValues([def.encabezados])
      .setBackground(AI27.COLOR)
      .setFontColor(AI27.COLOR_TEXTO)
      .setFontWeight('bold')
      .setWrap(true)
      .setVerticalAlignment('middle');

    sh.setFrozenRows(1);
    sh.getRange(1, 1, Math.max(AI27.FILAS, sh.getMaxRows()), columnas).createFilter();

    if (def.filas && def.filas.length) {
      sh.getRange(2, 1, def.filas.length, def.filas[0].length).setValues(def.filas);
    }
    if (preservedRows.length) {
      sh.getRange(2, 1, preservedRows.length, columnas).setValues(preservedRows);
    }

    (def.anchos || []).forEach((ancho, indice) => sh.setColumnWidth(indice + 1, ancho));
    sh.setRowHeight(1, 36);
    sh.setTabColor(rol === 'SUPERVISOR' ? '#4472C4' : '#70AD47');
  });
}

function AI27_cargarCatalogos_(ss) {
  const sh = ss.getSheetByName('Catalogo');
  if (!sh) throw new Error('No existe la hoja Catalogo en ' + ss.getName());

  const esSupervisor = ss.getId() === AI27.SPREADSHEET_ID_SUPERVISOR;
  const columnas = [
    AI27.ESTACIONES,
    AI27.ESTATUS,
    AI27.INCIDENCIAS
  ];
  if (esSupervisor) columnas.push(['Individual', 'Masivo']);
  columnas.push(AI27.CORREDORES, AI27.SERVICIOS, AI27.SUBTIPOS, AI27.VALIDADORES);

  const max = Math.max(...columnas.map(columna => columna.length));
  const matriz = Array.from({ length: max }, (_, fila) =>
    columnas.map(columna => columna[fila] || '')
  );

  sh.getRange(2, 1, max, columnas.length).setValues(matriz);
}

function AI27_aplicarValidaciones() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  if (!AI27_esLibroReconocido_(ss.getId())) {
    SpreadsheetApp.getUi().alert('Este libro no está configurado en AI27.');
    return;
  }
  AI27_aplicarValidacionesLibro_(ss);
  SpreadsheetApp.getUi().alert('Listas desplegables aplicadas.');
}

function AI27_aplicarValidacionesLibro_(ss) {
  const id = ss.getId();
  if (!AI27_esLibroReconocido_(id)) {
    throw new Error('ID de libro no reconocido: ' + id);
  }

  const catalogo = ss.getSheetByName('Catalogo');
  if (!catalogo) throw new Error('No existe la hoja Catalogo.');

  const reglaRango = (rango) => SpreadsheetApp.newDataValidation()
    .requireValueInRange(rango, true)
    .setAllowInvalid(false)
    .build();
  const estaciones = reglaRango(catalogo.getRange(2, 1, AI27.ESTACIONES.length, 1));
  const estatus = reglaRango(catalogo.getRange(2, 2, AI27.ESTATUS.length, 1));
  const incidencias = reglaRango(catalogo.getRange(2, 3, AI27.INCIDENCIAS.length, 1));
  const corredor = reglaRango(catalogo.getRange(2, esSupervisor ? 5 : 4, AI27.CORREDORES.length, 1));
  const servicio = reglaRango(catalogo.getRange(2, esSupervisor ? 6 : 5, AI27.SERVICIOS.length, 1));
  const subtipo = reglaRango(catalogo.getRange(2, esSupervisor ? 7 : 6, AI27.SUBTIPOS.length, 1));
  const validador = reglaRango(catalogo.getRange(2, esSupervisor ? 8 : 7, AI27.VALIDADORES.length, 1));
  const estadoValidacion = SpreadsheetApp.newDataValidation()
    .requireValueInList(AI27.ESTADOS_VALIDACION, true)
    .setAllowInvalid(false)
    .build();

  if (id === AI27.SPREADSHEET_ID_SUPERVISOR) {
    const base = ss.getSheetByName('Base de datos');
    const monitoreo = ss.getSheetByName('Monitoreo En tiempo real');
    const carga = ss.getSheetByName('CARGA DE BITÁCORAS');

    if (base) {
      base.getRange(2, 5, AI27.FILAS - 1, 1).setDataValidation(servicio);
      base.getRange(2, 6, AI27.FILAS - 1, 1).setDataValidation(subtipo);
      base.getRange(2, 15, AI27.FILAS - 1, 1).setDataValidation(corredor);
      base.getRange(2, 16, AI27.FILAS - 1, 1).setDataValidation(estaciones);
      base.getRange(2, 17, AI27.FILAS - 1, 1).setDataValidation(estatus);
    }
    if (monitoreo) {
      monitoreo.getRange(2, 1, AI27.FILAS - 1, 1).setDataValidation(estaciones);
      monitoreo.getRange(2, 3, AI27.FILAS - 1, 1).setDataValidation(servicio);
      monitoreo.getRange(2, 4, AI27.FILAS - 1, 1).setDataValidation(subtipo);
      monitoreo.getRange(2, 13, AI27.FILAS - 1, 1).setDataValidation(estatus);
      monitoreo.getRange(2, 14, AI27.FILAS - 1, 1).setDataValidation(incidencias);
      monitoreo.getRange(2, 15, AI27.FILAS - 1, 1).setDataValidation(corredor);
    }
    if (carga) {
      carga.getRange('B4').setDataValidation(
        SpreadsheetApp.newDataValidation()
          .requireValueInList(['Individual', 'Masivo'], true)
          .setAllowInvalid(false)
          .build()
      );
      carga.getRange('B5').setDataValidation(
        SpreadsheetApp.newDataValidation()
          .requireValueInList(['NO', 'SI'], true)
          .setAllowInvalid(false)
          .build()
      );
    }
    const validaciones = ss.getSheetByName('Validaciones');
    if (validaciones) {
      validaciones.getRange(2, 8, AI27.FILAS - 1, 1).setDataValidation(validador);
      validaciones.getRange(2, 11, AI27.FILAS - 1, 1).setDataValidation(estadoValidacion);
    }
  } else {
    AI27.ESTACIONES.forEach(nombre => {
      const sh = ss.getSheetByName(nombre);
      if (sh) {
        sh.getRange(2, 1, AI27.FILAS - 1, 1).setDataValidation(servicio);
        sh.getRange(2, 3, AI27.FILAS - 1, 1).setDataValidation(subtipo);
        sh.getRange(2, 12, AI27.FILAS - 1, 1).setDataValidation(estatus);
        sh.getRange(2, 13, AI27.FILAS - 1, 1).setDataValidation(incidencias);
        sh.getRange(2, 14, AI27.FILAS - 1, 1).setDataValidation(corredor);
      }
    });

    const validaciones = ss.getSheetByName('Validaciones');
    if (validaciones) {
      validaciones.getRange(2, 5, AI27.FILAS - 1, 1).setDataValidation(estaciones);
      validaciones.getRange(2, 8, AI27.FILAS - 1, 1).setDataValidation(validador);
      validaciones.getRange(2, 11, AI27.FILAS - 1, 1).setDataValidation(estadoValidacion);
    }
  }
}

function AI27_verificarEstructura() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const id = ss.getId();
  if (!AI27_esLibroReconocido_(id)) {
    SpreadsheetApp.getUi().alert('Este libro no está configurado en AI27.');
    return;
  }

  const defs = id === AI27.SPREADSHEET_ID_SUPERVISOR
    ? AI27_HOJAS_SUPERVISOR
    : AI27_HOJAS_MONITORISTA;
  const problemas = [];

  defs.forEach(def => {
    const sh = ss.getSheetByName(def.nombre);
    if (!sh) {
      problemas.push('Falta la hoja: ' + def.nombre);
      return;
    }
    const encabezados = sh.getRange(1, 1, 1, def.encabezados.length).getDisplayValues()[0];
    if (encabezados.some((valor, i) => valor !== def.encabezados[i])) {
      problemas.push('Encabezados incorrectos: ' + def.nombre);
    }
  });

  SpreadsheetApp.getUi().alert(
    problemas.length ? problemas.join('\n') : 'Estructura y encabezados correctos.'
  );
}

function AI27_mostrarTurnoActual() {
  const turno = AI27_obtenerTurno_(new Date());
  const hora = Utilities.formatDate(new Date(), AI27.ZONA, 'HH:mm');
  SpreadsheetApp.getUi().alert('Hora local: ' + hora + '\nTurno actual: ' + turno);
}

function AI27_obtenerTurno_(fecha) {
  const ahora = fecha || new Date();
  const partes = Utilities.formatDate(ahora, AI27.ZONA, 'HH:mm').split(':');
  const minutos = Number(partes[0]) * 60 + Number(partes[1]);
  const inicioNoche = AI27.CORTE_INICIO[0] * 60 + AI27.CORTE_INICIO[1];
  const inicioDia = AI27.CORTE_FIN[0] * 60 + AI27.CORTE_FIN[1];
  return minutos >= inicioDia && minutos < inicioNoche ? 'DIA' : 'NOCHE';
}

function AI27_procesarCarga() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  if (ss.getId() !== AI27.SPREADSHEET_ID_SUPERVISOR) {
    throw new Error('La carga debe ejecutarse desde SUPERVISOR.');
  }

  const sh = ss.getSheetByName('CARGA DE BITÁCORAS');
  const urlsTexto = String(sh.getRange('B3').getValue() || '').trim();
  const modo = String(sh.getRange('B4').getValue() || 'Individual').trim();
  const ejecutar = String(sh.getRange('B5').getValue() || '').trim().toUpperCase();

  if (ejecutar !== 'SI') {
    SpreadsheetApp.getUi().alert('En B5 selecciona SI para iniciar la carga.');
    return;
  }
  if (!urlsTexto) throw new Error('Pega al menos una URL en B3.');

  const urls = urlsTexto.split(/\n|,|;/).map(url => url.trim()).filter(Boolean);
  if (!['Individual', 'Masivo'].includes(modo)) throw new Error('Modo de carga no válido.');
  if (modo === 'Individual' && urls.length !== 1) {
    throw new Error('El modo Individual requiere exactamente una URL.');
  }

  sh.getRange('B6').setValue('Procesando...');
  let correctas = 0;
  const errores = [];

  urls.forEach(url => {
    try {
      const hojaGoogle = AI27_parsearUrlGoogleSheets_(url);
      if (hojaGoogle) {
        const resultado = AI27_importarBitacorasGoogle_(url, hojaGoogle);
        correctas += resultado.correctas;
        resultado.errores.forEach(mensaje => errores.push(url + ' : ' + mensaje));
        return;
      }

      AI27_guardarBitacora_(AI27_leerFichaAI27_(url), url);
      correctas++;
    } catch (error) {
      errores.push(url + ' : ' + error.message);
      AI27_error_('CARGA_FUENTE', error);
    }
  });

  sh.getRange('B6').setValue('Correctas: ' + correctas + ' | Errores: ' + errores.length);
  sh.getRange('B5').setValue('NO');
  SpreadsheetApp.getUi().alert(
    errores.length
      ? 'Carga terminada con observaciones:\n\n' + errores.join('\n')
      : 'Carga terminada. Registros procesados: ' + correctas
  );
}

function AI27_parsearUrlGoogleSheets_(url) {
  const texto = String(url || '').trim();
  const match = texto.match(
    /^https:\/\/docs\.google\.com\/spreadsheets\/d\/([A-Za-z0-9_-]+)(?:\/|$)/i
  );
  if (!match) return null;

  const gidMatch = texto.match(/[?&#]gid=(\d+)/i);
  return {
    id: match[1],
    gid: gidMatch ? Number(gidMatch[1]) : null
  };
}

function AI27_importarBitacorasGoogle_(url, referencia) {
  const libro = SpreadsheetApp.openById(referencia.id);
  let hoja = null;

  if (referencia.gid !== null) {
    hoja = libro.getSheets().find(item => item.getSheetId() === referencia.gid) || null;
    if (!hoja) throw new Error('No se encontró la pestaña indicada por gid=' + referencia.gid);
  } else {
    hoja = libro.getSheetByName('Bitacoras (16)') || libro.getSheets()[0];
  }
  if (!hoja) throw new Error('El libro de Google Sheets no contiene pestañas.');

  const filas = hoja.getDataRange().getDisplayValues();
  if (filas.length < 2) throw new Error('La pestaña no contiene registros para importar.');

  const normalizar = valor => String(valor || '')
    .replace(/\u00a0/g, ' ')
    .normalize('NFD')
    .replace(/[\u0300-\u036f]/g, '')
    .replace(/\s+/g, ' ')
    .trim()
    .toLowerCase();

  const encabezados = filas[0].map(normalizar);
  const nombres = {
    bitacora: 'numero de bitacora',
    cliente: 'cliente',
    servicio: 'tipo de monitoreo',
    linea: 'linea de transporte',
    eco: 'eco. transporte',
    placas: 'placas transporte',
    origen: 'origen',
    destino: 'destinos',
    telefono: 'telefono de contacto',
    operador: 'operadores'
  };
  const indices = {};
  const faltantes = [];

  Object.keys(nombres).forEach(campo => {
    indices[campo] = encabezados.indexOf(normalizar(nombres[campo]));
    if (indices[campo] < 0) faltantes.push(nombres[campo]);
  });
  if (faltantes.length) {
    throw new Error('Faltan encabezados requeridos: ' + faltantes.join(', '));
  }

  let correctas = 0;
  const errores = [];
  for (let i = 1; i < filas.length; i++) {
    const fila = filas[i];
    const bitacora = String(fila[indices.bitacora] || '').trim();
    if (!bitacora) continue;

    try {
      const telefonoOrigen = String(fila[indices.telefono] || '');
      const telefonoOperador = telefonoOrigen.match(/\(Op\)\s*([^,]+)/i);
      const datos = {
        bitacora: bitacora,
        cliente: String(fila[indices.cliente] || '').trim(),
        servicio: String(fila[indices.servicio] || '').trim(),
        linea: String(fila[indices.linea] || '').trim(),
        eco: String(fila[indices.eco] || '').trim(),
        placas: String(fila[indices.placas] || '').trim(),
        origen: String(fila[indices.origen] || '').trim(),
        destino: String(fila[indices.destino] || '').trim(),
        telefono: telefonoOperador ? telefonoOperador[1].trim() : '',
        operador: String(fila[indices.operador] || '').trim()
      };

      AI27_guardarBitacora_(datos, url);
      correctas++;
    } catch (error) {
      const mensaje = 'Fila ' + (i + 1) + ': ' + error.message;
      errores.push(mensaje);
      AI27_error_('CARGA_FUENTE_GOOGLE', error);
    }
  }

  if (correctas === 0 && errores.length === 0) {
    throw new Error('No se encontraron filas con número de bitácora.');
  }
  return { correctas: correctas, errores: errores };
}

function AI27_leerFichaAI27_(url) {
  if (!/^https:\/\/ai27services\.com\/DataSheet\/ficha-tecnica\//i.test(url)) {
    throw new Error('La URL no corresponde al formato de ficha AI27.');
  }

  const respuesta = UrlFetchApp.fetch(url, { muteHttpExceptions: true, followRedirects: true });
  if (respuesta.getResponseCode() !== 200) {
    throw new Error('La ficha respondió HTTP ' + respuesta.getResponseCode());
  }

  const html = respuesta.getContentText();
  if (!html || html.length < 100) throw new Error('La ficha no devolvió contenido legible.');

  const datos = {
    cliente: AI27_extraerCampoFichaHtml_(html, ['Cliente']),
    bitacora: AI27_extraerCampoFichaHtml_(html, ['Bitácora', 'Bitacora']),
    servicio: AI27_extraerCampoFichaHtml_(html, ['Tipo de monitoreo']),
    linea: AI27_extraerCampoFichaHtml_(html, ['Línea de transporte', 'Linea de transporte']),
    eco: AI27_extraerCampoFichaHtml_(html, ['Económico', 'Economico']),
    placas: AI27_extraerCampoFichaHtml_(html, ['Placa', 'Placas']),
    origen: AI27_extraerCampoFichaHtml_(html, ['Origen']),
    operador: AI27_extraerCeldaSeccionFicha_(html, 'Operadores', 0),
    telefono: AI27_extraerCeldaSeccionFicha_(html, 'Operadores', 1),
    destino: AI27_extraerCeldaSeccionFicha_(html, 'Destinos', 0)
  };

  if (!datos.bitacora) throw new Error('No se encontró la bitácora en el HTML de la ficha.');
  return datos;
}

function AI27_extraerCampoFichaHtml_(html, etiquetas) {
  for (const etiqueta of etiquetas) {
    const escaped = etiqueta.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
    const regex = new RegExp(
      '<b\\b[^>]*>\\s*' + escaped + '\\s*:?\\s*</b>([\\s\\S]*?)(?=<b\\b|</div>|</section>)',
      'i'
    );
    const match = html.match(regex);
    if (!match || !match[1]) continue;

    const valor = AI27_textoPlanoFicha_(match[1]);
    if (valor && !/^sin información|^sin operador|^no disponible|^n\/a$/i.test(valor)) {
      return valor;
    }
  }
  return '';
}

function AI27_extraerCeldaSeccionFicha_(html, titulo, indiceColumna) {
  const escaped = titulo.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
  const regex = new RegExp(
    '<section\\b[^>]*>\\s*<h4\\b[^>]*>\\s*' + escaped + '\\s*</h4>([\\s\\S]*?)</section>',
    'i'
  );
  const match = html.match(regex);
  if (!match) return '';

  const filas = match[1].match(/<tr\b[^>]*>[\s\S]*?<\/tr>/gi) || [];
  for (const fila of filas) {
    const celdas = fila.match(/<td\b[^>]*>[\s\S]*?<\/td>/gi) || [];
    if (celdas.length <= indiceColumna) continue;

    const contenido = celdas[indiceColumna]
      .replace(/^<td\b[^>]*>/i, '')
      .replace(/<\/td>$/i, '');
    const valor = AI27_textoPlanoFicha_(contenido);
    if (valor && !/^no hay datos que mostrar/i.test(valor)) return valor;
  }
  return '';
}

function AI27_textoPlanoFicha_(html) {
  return String(html || '')
    .replace(/<[^>]+>/g, ' ')
    .replace(/&nbsp;|&#160;/gi, ' ')
    .replace(/&#x([0-9a-f]+);/gi, (_, hex) => String.fromCharCode(parseInt(hex, 16)))
    .replace(/&#(\d+);/g, (_, dec) => String.fromCharCode(parseInt(dec, 10)))
    .replace(/&amp;/gi, '&')
    .replace(/&quot;/gi, '"')
    .replace(/&#39;/gi, "'")
    .replace(/&lt;/gi, '<')
    .replace(/&gt;/gi, '>')
    .replace(/\s+/g, ' ')
    .trim();
}

function AI27_guardarBitacora_(datos, url) {
  const ss = SpreadsheetApp.openById(AI27.SPREADSHEET_ID_SUPERVISOR);
  const sh = ss.getSheetByName('Base de datos');
  if (!sh) throw new Error('No existe la hoja Base de datos.');

  const existente = AI27_buscarFila_(sh, AI27_columnaEncabezado_(sh, 'Bitácora'), datos.bitacora);
  const ahora = new Date();
  const clasificacion = AI27_clasificarServicio_(datos);
  const fila = [
    ahora,
    Utilities.formatDate(ahora, AI27.ZONA, 'HH:mm:ss'),
    datos.cliente,
    datos.bitacora,
    clasificacion.servicio,
    clasificacion.subtipo,
    datos.servicio,
    datos.linea,
    datos.eco,
    datos.placas,
    datos.operador,
    datos.telefono,
    datos.origen || '',
    datos.destino,
    '',
    '',
    'Pendiente'
  ];

  if (existente) {
    const anterior = sh.getRange(existente, 1, 1, fila.length).getValues()[0];
    [4, 5, 14, 15, 16].forEach(indice => {
      if (fila[indice] === '' || fila[indice] === 'Pendiente') fila[indice] = anterior[indice];
    });
    sh.getRange(existente, 1, 1, fila.length).setValues([fila]);
  } else {
    sh.appendRow(fila);
  }
}

function AI27_clasificarServicio_(datos) {
  const normalizar = valor => String(valor || '')
    .normalize('NFD')
    .replace(/[\u0300-\u036f]/g, '')
    .toLowerCase()
    .trim();
  const tipo = normalizar(datos.servicio);
  const destino = normalizar(datos.destino);
  const contiene = (texto, opciones) => opciones.some(opcion => texto.indexOf(opcion) !== -1);

  if (contiene(tipo, ['inteligente con custodia'])) {
    return { servicio: 'Pirelli PPE', subtipo: 'Pago por evento inteligente con custodia' };
  }
  if (contiene(tipo, ['equipo original'])) {
    return { servicio: 'Equipo Original', subtipo: 'Equipo Original' };
  }
  if (contiene(tipo, ['distribucion'])) {
    return { servicio: 'Pirelli Distribución', subtipo: 'Distribución' };
  }
  if (contiene(tipo, ['exportacion aerea', 'expo aereo'])) {
    return { servicio: 'Exportación aérea', subtipo: 'Exportación aérea' };
  }
  if (contiene(tipo, ['exportacion maritima', 'expo maritimo'])) {
    return { servicio: 'Exportación marítima', subtipo: 'Exportación marítima' };
  }
  if (contiene(tipo, ['importacion'])) {
    return { servicio: 'Importación', subtipo: 'Importación' };
  }
  if (contiene(tipo, ['expo nl']) || contiene(destino, ['nuevo laredo'])) {
    return { servicio: 'Exportación NL', subtipo: 'Exportación Nuevo Laredo' };
  }
  if (contiene(tipo, ['expo slp']) || contiene(destino, ['san luis potosi'])) {
    return { servicio: 'Exportación SLP', subtipo: 'Exportación San Luis' };
  }
  if (contiene(tipo, ['expo tuxpan']) || contiene(destino, ['tuxpan'])) {
    return { servicio: 'Exportación Tuxpan', subtipo: 'Exportación Tuxpan' };
  }
  return { servicio: '', subtipo: '' };
}

function AI27_enviarSeleccionadasMonitoreo() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  if (ss.getId() !== AI27.SPREADSHEET_ID_SUPERVISOR) throw new Error('Ejecuta desde SUPERVISOR.');
  const sh = ss.getActiveSheet();
  if (sh.getName() !== 'Base de datos') throw new Error('Selecciona filas en Base de datos.');

  const rango = sh.getActiveRange();
  if (!rango || rango.getRow() < 2) throw new Error('Selecciona filas de datos.');
  const confirmacion = SpreadsheetApp.getUi().prompt(
    'Enviar a Monitoreo',
    'Escribe ENVIAR para confirmar.',
    SpreadsheetApp.getUi().ButtonSet.OK_CANCEL
  );
  if (confirmacion.getSelectedButton() !== SpreadsheetApp.getUi().Button.OK ||
      confirmacion.getResponseText().trim() !== 'ENVIAR') return;

  const mon = ss.getSheetByName('Monitoreo En tiempo real');
  const filas = sh.getRange(rango.getRow(), 1, rango.getNumRows(), 17).getValues();
  const colBitacoraMon = AI27_columnaEncabezado_(mon, 'Bitácora');
  let enviados = 0;
  let omitidos = 0;

  filas.forEach(fila => {
    const bitacora = String(fila[3] || '').trim();
    const estacion = String(fila[15] || '').trim();
    if (!bitacora || !AI27.ESTACIONES.includes(estacion) ||
        AI27_buscarFila_(mon, colBitacoraMon, bitacora)) {
      omitidos++;
      return;
    }
    mon.appendRow([
      estacion, fila[2], fila[4], fila[5], fila[6],
      fila[13], fila[9], bitacora, fila[7], fila[8], fila[10],
      '', fila[16] || 'Pendiente', '', fila[14], new Date(), 'NO'
    ]);
    enviados++;
  });

  SpreadsheetApp.getUi().alert(
    'Enviados: ' + enviados + '\nOmitidos, sin estación o duplicados: ' + omitidos
  );
}

function AI27_revisarTransferencias() {
  const ss = SpreadsheetApp.openById(AI27.SPREADSHEET_ID_SUPERVISOR);
  const base = ss.getSheetByName('Base de datos');
  const monitoreo = ss.getSheetByName('Monitoreo En tiempo real');
  if (!base || !monitoreo) throw new Error('Falta Base de datos o Monitoreo En tiempo real.');

  const colBase = AI27_columnaEncabezado_(base, 'Bitácora');
  const colMonitoreo = AI27_columnaEncabezado_(monitoreo, 'Bitácora');
  const idsBase = AI27_leerBitacoras_(base, colBase);
  const idsMonitoreo = AI27_leerBitacoras_(monitoreo, colMonitoreo);
  const enBaseNoMonitoreo = idsBase.filter(id => !idsMonitoreo.includes(id));
  const enMonitoreoNoBase = idsMonitoreo.filter(id => !idsBase.includes(id));

  const detalle = [
    'Base de datos: ' + idsBase.length,
    'Monitoreo activo: ' + idsMonitoreo.length,
    '',
    'En Base pero no en Monitoreo (' + enBaseNoMonitoreo.length + '):',
    enBaseNoMonitoreo.length ? enBaseNoMonitoreo.join(', ') : 'Ninguna',
    '',
    'En Monitoreo pero no en Base (' + enMonitoreoNoBase.length + '):',
    enMonitoreoNoBase.length ? enMonitoreoNoBase.join(', ') : 'Ninguna'
  ].join('\n');

  SpreadsheetApp.getUi().alert('Revisión de transferencias', detalle, SpreadsheetApp.getUi().ButtonSet.OK);
}

function AI27_columnaEncabezado_(sh, encabezado) {
  if (sh.getLastColumn() < 1) throw new Error('La hoja ' + sh.getName() + ' no tiene encabezados.');
  const normalizar = valor => String(valor || '')
    .replace(/\u00a0/g, ' ')
    .normalize('NFD')
    .replace(/[\u0300-\u036f]/g, '')
    .trim()
    .toLowerCase();
  const buscado = normalizar(encabezado);
  const encabezados = sh.getRange(1, 1, 1, sh.getLastColumn()).getDisplayValues()[0];
  const indice = encabezados.findIndex(valor => normalizar(valor) === buscado);
  if (indice < 0) throw new Error('No se encontró la columna "' + encabezado + '" en ' + sh.getName() + '.');
  return indice + 1;
}

function AI27_leerBitacoras_(sh, columna) {
  if (sh.getLastRow() < 2) return [];
  const valores = sh.getRange(2, columna, sh.getLastRow() - 1, 1).getDisplayValues();
  const ids = [];
  valores.forEach(fila => {
    const id = String(fila[0] || '').trim();
    if (id && !ids.includes(id)) ids.push(id);
  });
  return ids;
}

function AI27_publicarSeleccionadas() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  if (ss.getId() !== AI27.SPREADSHEET_ID_SUPERVISOR) throw new Error('Ejecuta desde SUPERVISOR.');
  const sh = ss.getActiveSheet();
  if (sh.getName() !== 'Monitoreo En tiempo real') throw new Error('Selecciona filas en Monitoreo En tiempo real.');

  const rango = sh.getActiveRange();
  if (!rango || rango.getRow() < 2) throw new Error('Selecciona filas para publicar.');
  const confirmacion = SpreadsheetApp.getUi().prompt(
    'Publicar a estaciones',
    'Escribe PUBLICAR para confirmar.',
    SpreadsheetApp.getUi().ButtonSet.OK_CANCEL
  );
  if (confirmacion.getSelectedButton() !== SpreadsheetApp.getUi().Button.OK ||
      confirmacion.getResponseText().trim() !== 'PUBLICAR') return;

  const monitorista = SpreadsheetApp.openById(AI27.SPREADSHEET_ID_MONITORISTA);
  const filas = sh.getRange(rango.getRow(), 1, rango.getNumRows(), 17).getValues();
  let publicados = 0;
  let omitidos = 0;

  filas.forEach((fila, indice) => {
    const filaHoja = rango.getRow() + indice;
    const estacion = String(fila[0] || '').trim();
    const bitacora = String(fila[7] || '').trim();
    if (!AI27.ESTACIONES.includes(estacion) || !bitacora) {
      omitidos++;
      return;
    }

    const destino = monitorista.getSheetByName(estacion);
    if (!destino) throw new Error('No existe la hoja ' + estacion + ' en Monitorista.');
    const colBitacoraDestino = AI27_columnaEncabezado_(destino, 'Bitácora');
    const existente = AI27_buscarFila_(destino, colBitacoraDestino, bitacora);
    const registro = [
      fila[2], fila[1], fila[3], fila[4], fila[5], fila[6], bitacora,
      fila[8], fila[9], fila[10], fila[11], fila[12] || 'Pendiente',
      fila[13], fila[14], new Date()
    ];

    if (existente) {
      const anterior = destino.getRange(existente, 1, 1, 15).getValues()[0];
      registro[10] = anterior[10];
      registro[11] = anterior[11];
      registro[12] = anterior[12];
      registro[14] = anterior[14];
      destino.getRange(existente, 1, 1, 15).setValues([registro]);
    } else {
      destino.appendRow(registro);
    }

    sh.getRange(filaHoja, 17).setValue('SI');
    publicados++;
  });

  SpreadsheetApp.getUi().alert('Publicados: ' + publicados + '\nOmitidos: ' + omitidos);
}

function AI27_sincronizarAvances() {
  const ss = SpreadsheetApp.openById(AI27.SPREADSHEET_ID_SUPERVISOR);
  const monitorista = SpreadsheetApp.openById(AI27.SPREADSHEET_ID_MONITORISTA);
  const mon = ss.getSheetByName('Monitoreo En tiempo real');
  if (!mon || mon.getLastRow() < 2) {
    SpreadsheetApp.getUi().alert('No hay servicios en Monitoreo.');
    return;
  }

  const filasMon = mon.getRange(2, 1, mon.getLastRow() - 1, 17).getValues();
  const avances = {};
  AI27.ESTACIONES.forEach(estacion => {
    const hoja = monitorista.getSheetByName(estacion);
    if (!hoja || hoja.getLastRow() < 2) return;
    hoja.getRange(2, 1, hoja.getLastRow() - 1, 15).getValues().forEach(fila => {
      const bitacora = String(fila[6] || '').trim();
      if (bitacora) avances[bitacora] = {
        reporte: fila[10], estatus: fila[11], incidencia: fila[12],
        corredor: fila[13], actualizado: fila[14], estacion: estacion
      };
    });
  });

  let actualizados = 0;
  const cierresPendientes = [];
  const incidenciasNuevas = [];
  filasMon.forEach((fila, indice) => {
    const avance = avances[String(fila[7] || '').trim()];
    if (!avance) return;
    const numeroFila = indice + 2;
    mon.getRange(numeroFila, 1).setValue(avance.estacion);
    mon.getRange(numeroFila, 12).setValue(avance.reporte);
    mon.getRange(numeroFila, 13).setValue(avance.estatus);
    mon.getRange(numeroFila, 14).setValue(avance.incidencia);
    mon.getRange(numeroFila, 15).setValue(avance.corredor);
    mon.getRange(numeroFila, 16).setValue(avance.actualizado);
    const registro = {
      bitacora: String(fila[7] || '').trim(),
      cliente: fila[1],
      servicio: fila[2],
      estacion: avance.estacion,
      incidencia: avance.incidencia,
      ultimoReporte: avance.reporte
    };
    if (avance.estatus === 'En destino') cierresPendientes.push(registro);
    if (AI27.INCIDENCIAS_CRITICAS.indexOf(String(avance.incidencia || '')) !== -1) {
      incidenciasNuevas.push(registro);
    }
    actualizados++;
  });

  cierresPendientes.forEach(AI27_encolarCierre_);
  incidenciasNuevas.forEach(AI27_registrarIncidencia_);
  AI27_actualizarDashboard();
  SpreadsheetApp.getUi().alert('Registros actualizados: ' + actualizados);
}

function AI27_cerrarSeleccionado() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const sh = ss.getActiveSheet();
  if (ss.getId() !== AI27.SPREADSHEET_ID_SUPERVISOR ||
      sh.getName() !== 'Cierres') {
    throw new Error('Selecciona un cierre pendiente en la hoja Cierres.');
  }

  const rango = sh.getActiveRange();
  if (!rango || rango.getRow() < 2) throw new Error('Selecciona un registro válido.');
  const fila = rango.getRow();
  const datos = sh.getRange(fila, 1, 1, 9).getValues()[0];
  if (String(datos[5] || '') !== 'Pendiente') {
    throw new Error('El cierre seleccionado ya fue procesado.');
  }
  const bitacora = String(datos[1] || '').trim();
  if (!bitacora) throw new Error('El cierre no contiene número de bitácora.');
  const ui = SpreadsheetApp.getUi();
  const quien = ui.prompt('Aprobar cierre', 'Nombre del supervisor que autoriza el cierre.', ui.ButtonSet.OK_CANCEL);
  if (quien.getSelectedButton() !== ui.Button.OK) return;
  const observaciones = ui.prompt('Observaciones', 'Observaciones del cierre.', ui.ButtonSet.OK_CANCEL);
  if (observaciones.getSelectedButton() !== ui.Button.OK) return;

  const fecha = new Date();
  const cerradoPor = quien.getResponseText().trim();
  const nota = observaciones.getResponseText().trim();
  datos[5] = 'Finalizado';
  datos[6] = fecha;
  datos[7] = cerradoPor;
  datos[8] = nota;
  sh.getRange(fila, 1, 1, 9).setValues([datos]);
  ss.getSheetByName('_Cerradas').appendRow(datos);

  const base = ss.getSheetByName('Base de datos');
  const filaBase = AI27_buscarFila_(base, AI27_columnaEncabezado_(base, 'Bitácora'), bitacora);
  if (filaBase) base.deleteRow(filaBase);
  const monitoreo = ss.getSheetByName('Monitoreo En tiempo real');
  const filaMonitoreo = AI27_buscarFila_(
    monitoreo,
    AI27_columnaEncabezado_(monitoreo, 'Bitácora'),
    bitacora
  );
  if (filaMonitoreo) monitoreo.deleteRow(filaMonitoreo);

  const monitorista = SpreadsheetApp.openById(AI27.SPREADSHEET_ID_MONITORISTA);
  AI27.ESTACIONES.forEach(estacion => {
    const hoja = monitorista.getSheetByName(estacion);
    const encontrada = AI27_buscarFila_(hoja, AI27_columnaEncabezado_(hoja, 'Bitácora'), bitacora);
    if (encontrada) hoja.deleteRow(encontrada);
  });
  ui.alert('Cierre aprobado; bitácora retirada de Base de datos y monitoreo: ' + bitacora);
}

function AI27_actualizarDashboard() {
  const ss = SpreadsheetApp.openById(AI27.SPREADSHEET_ID_SUPERVISOR);
  const mon = ss.getSheetByName('Monitoreo En tiempo real');
  const dash = ss.getSheetByName('Dashboard_Datos');
  if (!mon || !dash) throw new Error('Falta Monitoreo o Dashboard_Datos.');

  if (dash.getLastRow() > 1) {
    dash.getRange(2, 1, dash.getLastRow() - 1, 6).clearContent();
  }
  if (mon.getLastRow() < 2) return;

  const datos = mon.getRange(2, 1, mon.getLastRow() - 1, 17).getValues();
  const hoy = Utilities.formatDate(new Date(), AI27.ZONA, 'yyyy-MM-dd');
  const resultado = AI27.ESTACIONES.map(estacion => {
    const filas = datos.filter(fila => fila[0] === estacion);
    return [
      hoy,
      estacion,
      filas.length,
      filas.filter(fila => fila[12] === 'En ruta').length,
      filas.filter(fila => fila[12] === 'Detenida').length,
      filas.filter(fila => fila[12] === 'Finalizado').length
    ];
  });
  dash.getRange(2, 1, resultado.length, 6).setValues(resultado);
}

function AI27_buscarFila_(sh, columna, valor) {
  if (!sh || !valor || sh.getLastRow() < 2) return 0;
  const buscado = String(valor).trim();
  const valores = sh.getRange(2, columna, sh.getLastRow() - 1, 1).getDisplayValues();
  for (let indice = 0; indice < valores.length; indice++) {
    if (String(valores[indice][0]).trim() === buscado) return indice + 2;
  }
  return 0;
}

function AI27_esLibroReconocido_(id) {
  return id === AI27.SPREADSHEET_ID_SUPERVISOR ||
    id === AI27.SPREADSHEET_ID_MONITORISTA;
}

function AI27_error_(modulo, error) {
  try {
    const ss = SpreadsheetApp.openById(AI27.SPREADSHEET_ID_SUPERVISOR);
    let sh = ss.getSheetByName('_Errores');
    if (!sh) {
      sh = ss.insertSheet('_Errores');
      sh.appendRow(['Fecha y hora', 'Módulo', 'Función', 'Mensaje', 'Detalle']);
    }
    sh.appendRow([
      new Date(), modulo, 'AI27',
      error && error.message ? error.message : String(error),
      error && error.stack ? error.stack : ''
    ]);
  } catch (errorRegistro) {
    console.error(errorRegistro);
  }
}

function AI27_instalarDisparadores() {
  const actual = SpreadsheetApp.getActiveSpreadsheet();
  if (actual.getId() !== AI27.SPREADSHEET_ID_SUPERVISOR) {
    throw new Error('Instala las automatizaciones desde el libro del Supervisor.');
  }

  const ids = [
    AI27.SPREADSHEET_ID_SUPERVISOR,
    AI27.SPREADSHEET_ID_MONITORISTA
  ];
  const existentes = ScriptApp.getProjectTriggers();
  ids.forEach(id => {
    const yaExiste = existentes.some(trigger =>
      trigger.getHandlerFunction() === 'AI27_onEdit_' &&
      trigger.getTriggerSourceId() === id
    );
    if (!yaExiste) {
      ScriptApp.newTrigger('AI27_onEdit_')
        .forSpreadsheet(id)
        .onEdit()
        .create();
    }
  });
  SpreadsheetApp.getUi().alert('Automatizaciones de edición instaladas en ambos libros.');
}

function AI27_onEdit_(evento) {
  if (!evento || !evento.range || !evento.source) return;
  const rango = evento.range;
  const hoja = rango.getSheet();
  if (rango.getRow() < 2 || rango.getNumRows() !== 1 || rango.getNumColumns() !== 1) return;
  const nombre = hoja.getName();
  const activa = nombre === 'Monitoreo En tiempo real' ||
    AI27.ESTACIONES.indexOf(nombre) !== -1;
  if (!activa) return;

  const encabezados = hoja.getRange(1, 1, 1, hoja.getLastColumn()).getDisplayValues()[0];
  const campo = String(encabezados[rango.getColumn() - 1] || '').trim();
  const registro = AI27_leerRegistroActivo_(hoja, rango.getRow());
  if (!registro.bitacora) return;

  if (campo === 'Estatus' && String(rango.getDisplayValue()).trim() === 'En destino') {
    AI27_encolarCierre_(registro);
    return;
  }
  if (campo === 'Incidencia' && AI27.INCIDENCIAS_CRITICAS.indexOf(
    String(rango.getDisplayValue()).trim()
  ) !== -1) {
    AI27_registrarIncidencia_(registro);
  }
}

function AI27_leerRegistroActivo_(hoja, fila) {
  const encabezados = hoja.getRange(1, 1, 1, hoja.getLastColumn()).getDisplayValues()[0];
  const valores = hoja.getRange(fila, 1, 1, hoja.getLastColumn()).getValues()[0];
  const registro = {};
  encabezados.forEach((encabezado, indice) => {
    registro[String(encabezado).trim()] = valores[indice];
  });
  registro.bitacora = String(registro['Bitácora'] || '').trim();
  registro.cliente = registro['Cliente'] || '';
  registro.servicio = registro['Servicio'] || '';
  registro.estacion = registro['Estación'] || hoja.getName();
  registro.incidencia = registro['Incidencia'] || '';
  registro.ultimoReporte = registro['Último reporte'] || '';
  return registro;
}

function AI27_encolarCierre_(registro) {
  const supervisor = SpreadsheetApp.openById(AI27.SPREADSHEET_ID_SUPERVISOR);
  const cierres = supervisor.getSheetByName('Cierres');
  if (!cierres) throw new Error('No existe la hoja Cierres.');
  const colBitacora = AI27_columnaEncabezado_(cierres, 'Bitácora');
  const estadoCol = AI27_columnaEncabezado_(cierres, 'Estado cierre');
  const existePendiente = cierres.getLastRow() >= 2 &&
    cierres.getRange(2, 1, cierres.getLastRow() - 1, cierres.getLastColumn())
      .getValues().some(fila =>
        String(fila[colBitacora - 1]).trim() === registro.bitacora &&
        String(fila[estadoCol - 1]) === 'Pendiente'
      );
  if (!existePendiente) {
    cierres.appendRow([
      new Date(), registro.bitacora, registro.cliente, registro.servicio,
      registro.estacion, 'Pendiente', '', '', ''
    ]);
  }
  AI27_eliminarDeMonitoreoActivo_(registro.bitacora);
}

function AI27_eliminarDeMonitoreoActivo_(bitacora) {
  const supervisor = SpreadsheetApp.openById(AI27.SPREADSHEET_ID_SUPERVISOR);
  const monitor = supervisor.getSheetByName('Monitoreo En tiempo real');
  const filaMonitor = AI27_buscarFila_(
    monitor,
    AI27_columnaEncabezado_(monitor, 'Bitácora'),
    bitacora
  );
  if (filaMonitor) monitor.deleteRow(filaMonitor);

  const monitorista = SpreadsheetApp.openById(AI27.SPREADSHEET_ID_MONITORISTA);
  AI27.ESTACIONES.forEach(estacion => {
    const hoja = monitorista.getSheetByName(estacion);
    const fila = AI27_buscarFila_(
      hoja,
      AI27_columnaEncabezado_(hoja, 'Bitácora'),
      bitacora
    );
    if (fila) hoja.deleteRow(fila);
  });
}

function AI27_registrarIncidencia_(registro) {
  const supervisor = SpreadsheetApp.openById(AI27.SPREADSHEET_ID_SUPERVISOR);
  const monitorista = SpreadsheetApp.openById(AI27.SPREADSHEET_ID_MONITORISTA);
  const supervisorValidaciones = supervisor.getSheetByName('Validaciones');
  const monitoristaValidaciones = monitorista.getSheetByName('Validaciones');
  if (!supervisorValidaciones || !monitoristaValidaciones) {
    throw new Error('Falta la hoja Validaciones en uno de los libros.');
  }

  const bitacoraCol = AI27_columnaEncabezado_(supervisorValidaciones, 'Bitácora');
  const incidenciaCol = AI27_columnaEncabezado_(supervisorValidaciones, 'Incidencia');
  const estadoCol = AI27_columnaEncabezado_(supervisorValidaciones, 'Estado');
  const hayPendiente = supervisorValidaciones.getLastRow() >= 2 &&
    supervisorValidaciones.getRange(2, 1, supervisorValidaciones.getLastRow() - 1,
      supervisorValidaciones.getLastColumn()).getValues().some(fila =>
      String(fila[bitacoraCol - 1]) === registro.bitacora &&
      String(fila[incidenciaCol - 1]) === String(registro.incidencia) &&
      String(fila[estadoCol - 1]) === 'Pendiente'
    );
  if (hayPendiente) return;

  const datos = [
    new Date(), registro.bitacora, registro.cliente, registro.servicio,
    registro.estacion, registro.incidencia, registro.ultimoReporte,
    '', '', '', 'Pendiente', ''
  ];
  supervisorValidaciones.appendRow(datos);
  monitoristaValidaciones.appendRow(datos);
}

function AI27_resolverIncidenciaSeleccionada() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const hoja = ss.getActiveSheet();
  if (hoja.getName() !== 'Validaciones') {
    throw new Error('Selecciona una fila de la hoja Validaciones.');
  }
  const fila = hoja.getActiveRange() ? hoja.getActiveRange().getRow() : 0;
  if (fila < 2) throw new Error('Selecciona una incidencia válida.');

  const encabezados = hoja.getRange(1, 1, 1, hoja.getLastColumn()).getDisplayValues()[0];
  const valores = hoja.getRange(fila, 1, 1, hoja.getLastColumn()).getValues()[0];
  const indice = nombre => encabezados.indexOf(nombre);
  const estadoIndex = indice('Estado');
  if (estadoIndex < 0 || String(valores[estadoIndex]) !== 'Pendiente') {
    throw new Error('La incidencia seleccionada no está pendiente.');
  }

  const ui = SpreadsheetApp.getUi();
  const quien = ui.prompt(
    'Resolver incidencia',
    'Indica quién validó: ' + AI27.VALIDADORES.join(', '),
    ui.ButtonSet.OK_CANCEL
  );
  if (quien.getSelectedButton() !== ui.Button.OK) return;
  const validador = quien.getResponseText().trim();
  if (AI27.VALIDADORES.indexOf(validador) === -1) {
    throw new Error('El validador debe coincidir con una opción del catálogo.');
  }
  const observaciones = ui.prompt('Observaciones', 'Detalle de la validación.', ui.ButtonSet.OK_CANCEL);
  if (observaciones.getSelectedButton() !== ui.Button.OK) return;

  const ahora = new Date();
  const inicio = new Date(valores[indice('Fecha inicio')]);
  const minutos = Math.max(0, Math.round((ahora.getTime() - inicio.getTime()) / 60000));
  const bitacora = String(valores[indice('Bitácora')] || '').trim();
  const incidencia = String(valores[indice('Incidencia')] || '').trim();
  const supervisor = SpreadsheetApp.openById(AI27.SPREADSHEET_ID_SUPERVISOR);
  const historial = supervisor.getSheetByName('Validaciones');
  const cols = encabezados;
  const filaHistorial = AI27_buscarValidacionPendiente_(historial, bitacora, incidencia);
  if (!filaHistorial) throw new Error('No se encontró la incidencia pendiente en el historial del Supervisor.');
  const set = (nombre, valor) => historial.getRange(
    filaHistorial, cols.indexOf(nombre) + 1
  ).setValue(valor);
  set('Validado por', validador);
  set('Fecha validación', ahora);
  set('Minutos transcurridos', minutos);
  set('Estado', 'Validada');
  set('Observaciones', observaciones.getResponseText().trim());

  const monitorista = SpreadsheetApp.openById(AI27.SPREADSHEET_ID_MONITORISTA);
  const validacionesMonitor = monitorista.getSheetByName('Validaciones');
  const filaMonitor = AI27_buscarValidacionPendiente_(validacionesMonitor, bitacora, incidencia);
  if (filaMonitor) validacionesMonitor.deleteRow(filaMonitor);
  AI27_limpiarIncidenciaActiva_(bitacora);
  ui.alert('Incidencia validada; tiempo transcurrido: ' + minutos + ' minutos.');
}

function AI27_buscarValidacionPendiente_(hoja, bitacora, incidencia) {
  if (!hoja || hoja.getLastRow() < 2) return 0;
  const encabezados = hoja.getRange(1, 1, 1, hoja.getLastColumn()).getDisplayValues()[0];
  const bitacoraCol = encabezados.indexOf('Bitácora') + 1;
  const incidenciaCol = encabezados.indexOf('Incidencia') + 1;
  const estadoCol = encabezados.indexOf('Estado') + 1;
  const filas = hoja.getRange(2, 1, hoja.getLastRow() - 1, hoja.getLastColumn()).getValues();
  for (let i = filas.length - 1; i >= 0; i--) {
    if (String(filas[i][bitacoraCol - 1]) === bitacora &&
        String(filas[i][incidenciaCol - 1]) === incidencia &&
        String(filas[i][estadoCol - 1]) === 'Pendiente') return i + 2;
  }
  return 0;
}

function AI27_limpiarIncidenciaActiva_(bitacora) {
  const supervisor = SpreadsheetApp.openById(AI27.SPREADSHEET_ID_SUPERVISOR);
  const monitor = supervisor.getSheetByName('Monitoreo En tiempo real');
  AI27_limpiarIncidenciaFila_(monitor, bitacora);
  const monitorista = SpreadsheetApp.openById(AI27.SPREADSHEET_ID_MONITORISTA);
  AI27.ESTACIONES.forEach(estacion =>
    AI27_limpiarIncidenciaFila_(monitorista.getSheetByName(estacion), bitacora)
  );
}

function AI27_limpiarIncidenciaFila_(hoja, bitacora) {
  if (!hoja) return;
  const fila = AI27_buscarFila_(
    hoja,
    AI27_columnaEncabezado_(hoja, 'Bitácora'),
    bitacora
  );
  if (fila) {
    hoja.getRange(fila, AI27_columnaEncabezado_(hoja, 'Incidencia')).setValue('Sin incidencia');
  }
}
