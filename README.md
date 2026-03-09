display.setDefault("background", 0, 0.5, 1)

-- Título
local titulo = display.newText("Calculadora de IMC", display.contentCenterX, 40, native.systemFontBold, 28)

-- Campo peso
local peso = native.newTextField(display.contentCenterX, 100, 220, 40)
peso.placeholder = "Peso (Kg)"

-- Campo altura
local altura = native.newTextField(display.contentCenterX, 160, 220, 40)
altura.placeholder = "Altura (m)"

-- Resultado
local resultado = display.newText("IMC: --", display.contentCenterX, 220, native.systemFontBold, 24)

-- Media rueda
local rueda = display.newCircle(display.contentCenterX, 320, 80)
rueda:setFillColor(0.8)

-- Rectángulo para tapar la mitad y que parezca media rueda
local tapa = display.newRect(display.contentCenterX, 360, 200, 80)
tapa:setFillColor(0,0.5,1)

-- Texto dentro de la rueda
local textoIMC = display.newText("--", display.contentCenterX, 310, native.systemFontBold, 26)

-- Función para calcular IMC
local function calcularIMC()

    if peso.text == "" or altura.text == "" then
        resultado.text = "Completa los campos"
        return
    end

    local p = tonumber(peso.text)
    local h = tonumber(altura.text)

    if not p or not h then
        resultado.text = "Solo números"
        return
    end

    local imc = p / (h*h)
    imc = math.floor(imc * 10 + 0.5) / 10

    local categoria = ""

    if imc < 18.5 then
        categoria = "Bajo peso"
        rueda:setFillColor(1,1,0) -- amarillo

    elseif imc < 25 then
        categoria = "Normal"
        rueda:setFillColor(0,1,0) -- verde

    elseif imc < 30 then
        categoria = "Sobrepeso"
        rueda:setFillColor(1,0.5,0) -- naranja

    else
        categoria = "Obesidad"
        rueda:setFillColor(1,0.4,0.7) -- rosado
    end

    resultado.text = "IMC: "..imc.." ("..categoria..")"
    textoIMC.text = imc
end

-- Función limpiar
local function limpiar()
    peso.text = ""
    altura.text = ""
    resultado.text = "IMC: --"
    textoIMC.text = "--"
end

-- Botón calcular
local btnCalcular = display.newText("Calcular IMC", display.contentCenterX, 440, native.systemFontBold, 24)
btnCalcular:setFillColor(0,1,0)
btnCalcular:addEventListener("tap", calcularIMC)

-- Botón limpiar
local btnLimpiar = display.newText("Limpiar", display.contentCenterX, 480, native.systemFontBold, 22)
btnLimpiar:setFillColor(1,1,0)
btnLimpiar:addEventListener("tap", limpiar)
