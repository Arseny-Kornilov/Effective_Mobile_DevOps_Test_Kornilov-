# Effective Mobile DevOps Тестовое задание
## Cкрипт 
`
#!/bin/bash

# Проверка, что процесс запущен
if pgrep -x "test" > /dev/null; then

    if [ -f /tmp/test_running ]; then

        true
    else
        # Перезапуск
        echo "[$(date '+%Y-%m-%d %H:%M:%S')] Процесс test перезапущен" >> /var/log/monitoring.log
        
        touch /tmp/test_running
    fi
    
    # Процесс запущен, проверка доступности сервера
    if curl -s -f --max-time 5 https://test.com/monitoring/test/api > /dev/null 2>&1; then
        # Сервер доступен
        true
    else
        # Сервер недоступен
        echo "[$(date '+%Y-%m-%d %H:%M:%S')] Сервер мониторинга недоступен" >> /var/log/monitoring.log
    fi
else
    # Процесс не запущен
    rm -f /tmp/test_running 2>/dev/null
fi`
