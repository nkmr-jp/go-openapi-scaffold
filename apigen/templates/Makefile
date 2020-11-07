start:
	go mod vendor
	go run main.go

doc:
	open ./api/redoc-static.html

logs:
ifdef LEVEL
	tail -fq ./log/*.log | jq -R 'fromjson? | select(.level=="$(LEVEL)")'
else
	tail -fq ./log/*.log | jq -R 'fromjson?'
endif

inspect:
	@echo "${GREEN}# INSPECT: Last Error Log${RESET}"
	@cat ./log/*.log | jq -c 'select(.stacktrace)' | tail -n 1 | jq -r .
	@echo "${GREEN}# INSPECT: Stacktrace ${RESET}"
	@cat ./log/*.log | jq -c  'select(.stacktrace)' | tail -n 1 | jq -r .stacktrace
