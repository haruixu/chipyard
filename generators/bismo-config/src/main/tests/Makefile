MMIO_SIM_PATH = ../../../../../../sims/verilator/simulator-chipyard-BISMOTLRocketConfig # had to move back a dir to match the changes in the make goals
MMIO_SIM_TINY_PATH = ../../../../../../sims/verilator/simulator-chipyard-TinyBISMOConfig
MMIO_SIM_DEBUG_PATH = ../../../../../../sims/verilator/simulator-chipyard-BISMOTLRocketConfig-debug
ROCC_SIM_PATH = ../../../../../sims/verilator/simulator-chipyard-BISMOTLRocketConfig
ROCC_SIM_DEBUG_PATH = ../../../../../sims/verilator/simulator-chipyard-BISMOTLRocketConfig-debug

GCC=riscv64-unknown-linux-gnu-g++
# GCC=riscv64-linux-gnu-g++
GCC_FLAGS= -static


.DEFAULT_GOAL:=mmio-sw

mmio-sw:
	cd mmio && $(GCC) $(GCC_FLAGS) main.cpp -o main
	test -d mmio-out || mkdir mmio-out
	cd mmio && time $(MMIO_SIM_PATH) --cycle-count pk ./main 2>&1 | tee ../mmio-out/bismo-log-clock.txt
#.PHONY: overrides default file system behaviour where make tries to associate the command with a file. .PHONY tells make to do this association, thereby focusing on the goal

mmio-sw-tiny:
	cd mmio && $(GCC) $(GCC_FLAGS) main.cpp -o main
	test -d mmio-out || mkdir mmio-out
	cd mmio && time $(MMIO_TINY_SIM_PATH) --cycle-count pk ./main 2>&1 | tee ../mmio-out/bismo-log-clock-tiny.txt

test:
	cd mmio && riscv64-linux-gnu-g++ -static test.cpp -o test
	test -d mmio-out-test || mkdir mmio-out-test
	cd mmio && time $(MMIO_SIM_DEBUG_PATH) --cycle-count pk ./test 2>&1 | tee ../mmio-out-test/bismo-log-clock.txt

mmio-sw-debug:
	cd mmio && riscv64-linux-gnu-g++ -static main.cpp -o main
	test -d mmio-debug-out || mkdir mmio-debug-out
	cd mmio && time $(MMIO_SIM_DEBUG_PATH) --cycle-count --vcd=../mmio-debug-out/bismo-debug-clock.vcd pk ./main 2>&1 | tee ../mmio-debug-out/bismo-log-clock.txt

rocc-sw:
	cd rocc
	riscv64-unknown-linux-gnu-g++ -static main.cpp -o main
	test -d rocc-out || mkdir rocc-out
	time $(ROCC_SIM_PATH) --cycle-count --vcd=rocc-out/bismo-debug-clock.vcd pk ./main 2>&1 | tee rocc-out/bismo-log-clock.txt

rocc-sw-debug:
	cd rocc
	riscv64-unknown-linux-gnu-g++ -static main.cpp -o main
	test -d rocc-debug-out || mkdir rocc-debug-out
	time $(ROCC_SIM_DEBUG_PATH) --cycle-count --vcd=rocc-debug-out/out/bismo-debug-clock.vcd pk ./main 2>&1 | tee rocc-debug-out/bismo-log-clock.txt
