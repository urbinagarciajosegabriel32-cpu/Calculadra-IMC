display.setDefault("background", 0, 0.5, 1)

-- Título
local titulo = display.newText({
    text = "Calculadora de IMC",
    x = display.contentCenterX,
    y = 40,
    font = native.systemFontBold,
    fontSize = 28
})
titulo:setFillColor(1)

-- Campo peso
local peso = native.newTextField(display.contentCenterX, 100, 220, 40)
peso.placeholder = "Peso (Kg)"
peso.inputType = "decimal"

-- Campo altura
local altura = native.newTextField(display.contentCenterX, 160, 220, 40)
altura.placeholder = "Altura (m)"
altura.inputType = "decimal"

-- Resultado
local resultado = display.newText({
    text = "IMC: --",
    x = display.contentCenterX,
    y = 230,
    font = native.systemFont,
    fontSize = 22
})
resultado:setFillColor(1)

-- Función calcular IMC
local function calcularIMC()

    -- Validar campos vacíos
    if peso.text == "" or altura.text == "" then
        resultado.text = "Completa todos los campos"
        return
    end

    local p = tonumber(peso.text)
    local h = tonumber(altura.text)

    -- Validar números
    if not p or not h then
        resultado.text = "Ingresa solo números"
        return
    end

    -- Validar valores negativos
    if p <= 0 or h <= 0 then
        resultado.text = "Valores inválidos"
        return
    end

    -- Validar valores irreales
    if p < 20 or p > 300 then
        resultado.text = "Peso fuera de rango"
        return
    end

    if h < 1 or h > 2.5 then
        resultado.text = "Altura fuera de rango"
        return
    end

    -- Calcular IMC
    local imc = p / (h * h)

    -- Redondear a 1 decimal
    imc = math.floor(imc * 10 + 0.5) / 10

    -- Clasificación
    local categoria = ""

    if imc < 18.5 then
        categoria = "Bajo peso"
    elseif imc < 25 then
        categoria = "Normal"
    elseif imc < 30 then
        categoria = "Sobrepeso"
    else
        categoria = "Obesidad"
    end

    resultado.text = "IMC: " .. imc .. " (" .. categoria .. ")"
end

-- Función limpiar
local function limpiar()
    peso.text = ""
    altura.text = ""
    resultado.text = "IMC: --"
end

-- Botón calcular
local btnCalcular = display.newText({
    text = "Calcular IMC",
    x = display.contentCenterX,
    y = 290,
    font = native.systemFontBold,
    fontSize = 24
})
btnCalcular:setFillColor(0, 1, 0)
btnCalcular:addEventListener("tap", calcularIMC)

-- Botón limpiar
local btnLimpiar = display.newText({
    text = "Limpiar",
    x = display.contentCenterX,
    y = 340,
    font = native.systemFontBold,
    fontSize = 22
})
btnLimpiar:setFillColor(1, 1, 0)
btnLimpiar:addEventListener("tap", limpiar)
