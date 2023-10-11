# 개 요
망간 데이터 전송을 용이하게 하는 자료교환시스템의 잠재적 취약성을 알아보고 발생 가능한 위험에 대해 고찰합니다.

# 자료교환시스템 기능
자료교환시스템은 기본적으로 ‘파일 전송’ 기능과 ‘클립보드 전송’ 기능을 제공합니다.

[그림]

파일 전송 방식은 외부▸내부로 악성코드 유입 또는 내부▸외부로 중요문서를 유출하는 통로가 될 수 있으므로 관리자의 승인과 결재를 요구합니다. 반면 클립보드 전송 방식은 단순 텍스트만을 전송하므로 상대적으로 안전한 전송 방식으로 취급되어 별도의 승인 및 결재를 요구하지 않습니다.

[도식]

하지만 자료교환시스템 앞단에 특별한 용도로 제작된 전처리기를 배치시키면 안전하다고 여겨지던 클립보드 전송 방식에 맹점이 발생하게 되며, 이를 이용해 결재 승인 없이 파일을 자유롭게 주고받을 수 있게 됩니다.

[도식]

# 전처리기(FileExpress.exe)
전처리기는 발신 측에서 파일의 raw 데이터를 분석해 비트 단위로 해체(disassemble)하고 해체된 정보를 문자화시켜 운영체제의 클립보드 공간으로 보냅니다. 자료교환시스템에서 클립보드 전송을 수행하면 수신 측은 전달받은 문자 형태의 정보를 파일의 raw 데이터 값으로 인식되도록 하는 재조립(reassemble) 과정을 거쳐 원래 파일을 생성해냅니다.

[그림]

# 위험성
자료교환시스템에 접근할 수 있는 사용자, 협력사 직원, 권한이 낮은 직원 등이 악의적 목적으로 인터넷망▸업무망으로 제로데이 관련 악성코드를 유포하거나 업무망▸인터넷망으로 대용량 정보를 유출시키는 우회 통로로 사용될 여지가 있습니다. (일반 텍스트 데이터를 클립보드로 전송하는 것과 달리 평문이 아니므로 악성코드 및 개인정보를 전송 도중 탐지하기 어려움) 또한 암호화된 압축 파일, 실행 파일 등 자료교환시스템에서 허용되지 않는 파일도 자유롭게 주고받을 수 있게 됩니다.

# 대응방안
클립보드 전송 기능을 제한하는 것 외에 완벽한 대책이 존재하지 않을 것으로 사료됩니다. 예컨대 전송 구간에서 정규표현식이나 바이너리 패턴(헥사값)을 이용해 검사하더라도 해당 raw 데이터는 전송 시점에 이미 문자 데이터로 변환된 상태이며 해당 문자 데이터는 또 다른 헥사값을 가질 뿐이므로 탐지 장비에서 탐지되지 않습니다. 간단히 말하자면 헥사값의 헥사값이므로 탐지될 수 없음을 의미합니다. 정규 표현식을 이용한 탐지 방법도 마찬가지로 전송 시점에 평문이 아닌 바이너리 패턴으로 전송되므로 적절한 검사가 이뤄지기 어렵습니다. 부가적으로 전처리기에서 치환 기법, 인코딩 등을 이용하여 더 복잡한 난독화 과정을 거친다면 사실상 탐지는 불가능에 가깝습니다.

[그림]

아래는 몇 가지 고려할만한 대응 방법입니다.
 ● 클립보드 전송시 결재 기능 도입
 ● 클립보드 전송 용량 제한
 ● 단시간에 매우 빈번한 클립보드 전송이 이뤄질 경우 사용 제한
