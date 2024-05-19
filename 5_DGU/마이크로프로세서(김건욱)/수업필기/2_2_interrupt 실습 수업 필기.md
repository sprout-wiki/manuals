- 실습과제1 의 방식은 Polling 임
    - while 안에 switch 를 check 함
- interrupt service rutine (ISR) 로 구현

## 1) 외부 인터럽트

- 외부 신호가 발생시키는 인터럽트.
- 버튼 / 모터 엔코더 / 초음파 센서 등

### PxIES : Port X Interrupt Edge Select Resister

Bit 0 : low-to-high (Rising Edge) → PxIES flag set 1

Bit 1 : high-to-low (Falling Edge) → PxIES flag set 1

PxIFG : Port X Interrupt Flag Register

→ set 1 when intrerrupted

## 2) 내부 인터럽트

### Timer

- 2^16 -1 보다 커지면 overflow
- mode 가 있음
    - Up : overflow 에 도달하면 0 으로 초기화
    - Up/Down : 올라갔다가 내려감

### Pseudo Code