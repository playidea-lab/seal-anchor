# seal-anchor — SEAL 원장 head 앵커

이 저장소는 SEAL 증거 원장의 **head 서명**을 주기적으로 받는 공개 채널입니다.

## 왜 필요한가

원장은 append-only 해시체인이라 내용을 고치면 성적서 검증이 잡습니다.
그런데 **꼬리를 잘라내는 것은 잡히지 않습니다** — 잘라낸 뒤 남은 부분은
여전히 정합합니다.

그래서 head `(seq, head_hash)`를 운영자 통제 밖 채널로 내보냅니다.
여기 seq 100까지 있으면, 운영자는 seq 100 이전을 잘라낼 수 없습니다.
**간격의 구멍은 그 자체로 변조의 증거입니다.**

## 검증 방법

```
git clone https://github.com/playidea-lab/seal-anchor.git
seal anchors seal-anchor/anchors.jsonl --key jYDMAJV7tuIvzF2Z2/VDtzO6Pj7ecr7JrXtrjsuirfk=
```

성적서를 받았다면 그 구간이 덮이는지 함께 확인하십시오:

```
seal anchors seal-anchor/anchors.jsonl --key jYDMAJV7tuIvzF2Z2/VDtzO6Pj7ecr7JrXtrjsuirfk= --cert <성적서>.json
```

성적서가 쓰는 최고 seq보다 앵커가 낮으면, **그 구간이 공개 채널에 나간 적이
없어 절단을 배제할 수 없습니다.**

## 앵커 공개키

```
jYDMAJV7tuIvzF2Z2/VDtzO6Pj7ecr7JrXtrjsuirfk=
```

key_id `97bc9985493e5115`

이 키는 원장 서버가 아닌 곳에서도 확인할 수 있어야 의미가 있습니다 —
발급자에게 받은 키로 발급자를 검증하는 것은 검증이 아닙니다.

## 앵커가 보증하지 않는 것

앵커는 **절단을 배제할 뿐**입니다. 원장 내용이 정직한지, 등급이 맞는지,
시험이 봉인 뒤에 열렸는지는 성적서 검증(`seal verify`)이 봅니다.
