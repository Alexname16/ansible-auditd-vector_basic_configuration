Ansible: Auditd + Vector (Basic Security Configuration)

Ansible playbook для автоматизации базовых настроек безопасности на **Ubuntu 26.04**.  

Результат работы
  После выполнения playbook на целевом хосте:
  auditd пишет логи в /var/log/audit/audit.log с правилами:
  Мониторинг execve, fork, clone, clone3
  Контроль файлов /etc/passwd, /etc/shadow, /etc/sudoers
  Отслеживание запуска python3 не-root пользователями
  Vector агрегирует события и создаёт:
  /var/log/vector/vector_execve.log - сгруппированные события execve с цепочками предков (ancestor_chain, chain_depth)
  /var/log/vector/vector_non_execve.log - остальные события аудита
  /var/log/vector/vector_combined.log - все события из двух файлов

Конфигурация Vector использует aggregate с буфером 10 секунд для связывания событий по pid/ppid.
Глубина цепочки предков (ancestor_chain) ограничена 3 событиями.
Playbook идемпотентен - можно запускать повторно без побочных эффектов.
