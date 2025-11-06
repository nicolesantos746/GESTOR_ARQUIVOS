# GESTOR_ARQUIVOS

@echo off

:: Criação de Pastas

mkdir c:\GestorArquivos
cd c:\GestorArquivos
mkdir c:\GestorArquivos\Documentos
mkdir c:\GestorArquivos\Logs
mkdir c:\GestorArquivos\Backups
pause

:: Criação de Arquivos

echo Aqui entra o relatorio de uso >> c:\GestorArquivos\Documentos\relatorio.txt
echo Aqui fica os dado de login >> c:\GestorArquivos\Documentos\dados.csv
echo Aqui fica a configuração de login >> c:\GestorArquivos\Documentos\config.in
pause

:: Geração do Arquivo Log
echo off
title Registro de Operações 
set "log_dir=C:\GestorArquivos\Logs"
set "log_file=%log_dir%\logderegistro.txt"
set "operacao=Verificação de Sistema (Estrutura IF/GOTO)"
echo Executando %operacao%...
timeout /t 2 >nul

set /a errorlevel=0

if %errorlevel%==0 goto sucesso
goto falha

:sucesso
set "status=APROVADO"
goto registrar

:falha
set "status=FALHA (código %errorlevel%)"
goto registrar

:registrar
echo.
echo Tentando registrar a operação...


if exist "%log_dir%\" goto diretorio_existe
    
    echo O diretório de log "%log_dir%" não existe. Tentando criar...
    mkdir "%log_dir%"
    
    
    if errorlevel 1 goto erro_criar_diretorio
    
    echo Diretório "%log_dir%" criado com sucesso.
    goto continuar_registro
    
:diretorio_existe
    echo O diretório "%log_dir%" já existe.
    goto continuar_registro
    
:erro_criar_diretorio
    echo ERRO: Falha ao criar o diretório de log "%log_dir%". O registro NÃO será salvo em arquivo.
    goto exibir_somente

:continuar_registro

echo [%date% %time%] Operacao: %operacao% - Status: %status% >> "%log_file%"
echo Registro salvo em "%log_file%"

:exibir_somente
echo.
echo Resultado: %status%
echo Pressione qualquer tecla para sair...
pause >nul
exit /b

Title data e hora dos documentos 
echo [%date% - %time%] Operação: %operacao% - Resultado: %status% >> "%log%"
echo
echo Operação: %operacao%
echo Resultado: %status%
echo Log salvo em: %log%
echo
pause
exit /b

:: Fazendo o Backup

title Backup de Arquivos
set "origem=C:\GestorArquivos\Documentos"
set "destino=C:\GestorArquivos\Backups"
echo
echo   Iniciando Backup com Robocopy...
echo 
robocopy "%origem%" "%destino%" /E /COPYALL /R:2 /W:2 /LOG:"%destino%\log_backup.txt"
echo
echo   Backup concluído!
echo   Verifique o log em "%destino%\log_backup.txt"
echo 
pause

: : Relatorio de execuaço 
echo RELATÓRIO DE EXECUÇÃO > C:\GestorArquivos\resumo_execucao.txt
echo - >> C:\GestorArquivos\resumo_execucao.txt
echo Total de arquivos criados: X >> C:\GestorArquivos\resumo_execucao.txt
echo Total de pastas criadas: Y >> C:\GestorArquivos\resumo_execucao.txt
echo Data/Hora do backup: %date% %time% >> C:\GestorArquivos\resumo_execucao.txt
