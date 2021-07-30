소개
모든 창의적인 프로젝트의 필수 측면은 디버깅을 쉽게 만들고 코드를 조정하는 것입니다. 개발자(귀하)와 프로젝트에서 작업하는 다른 행위자(예: 디자이너 또는 클라이언트)는 가능한 한 많은 매개변수를 변경할 수 있어야 합니다.

최고의 경험을 위해 완벽한 색상, 속도, 수량 등을 찾으려면 이를 고려해야 합니다. 멋지게 보이는 예기치 않은 결과를 얻을 수도 있습니다.

먼저 디버그 UI가 필요합니다.

HTML / CSS / JS를 사용하여 자신만의 디버그 UI를 만들 수 있지만 이미 여러 라이브러리가 있습니다.

dat.GUI
제어판
컨트롤킷
의일
조정창
가이파이
위의
이 모든 것이 우리가 원하는 것을 할 수 있지만 가장 인기 있는 dat.GUI 를 사용할 것 입니다. 다른 것을 자유롭게 시도하십시오.

예
내 포트폴리오에서 디버그 UI의 좋은 예를 찾을 수 있습니다. 이 UI #debug는 URL에 추가할 때만 표시됩니다 .

https://bruno-simon.com/#debug

/assets/lessons/10/debug-1.png

중력, 색상, 속도, 요소 위치 등을 조정할 수 있습니다.

이러한 모든 조정을 만드는 데 많은 시간이 걸렸지만, 조정이 없으면 게임의 균형이 덜 잡힌 것처럼 보입니다.

설정
처음에는 큐브가 있지만 종속성은 Dat.GUI를 포함하지 않습니다. 우리는 그것을 추가하고 약간의 조정을 만들 것입니다.

/assets/lessons/10/step-01.png

Dat.GUI를 구현하는 방법
Webpack 프로젝트에 Dat.GUI를 추가하기 위해 NPM이라고 하는 Node.js와 함께 제공되는 종속성 관리자를 사용할 수 있습니다(이전 강의에서 GSAP에 대해 했던 것처럼).

터미널에서(서버가 실행되지 않거나 동일한 폴더의 다른 터미널 창을 사용하여) 실행 npm install --save dat.gui

이제 Dat.GUI를 /node_modules/폴더 에서 사용할 수 있으며 script.js. 서버를 다시 시작하는 것을 잊지 마십시오.

import './style.css'
import _ as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
import gsap from 'gsap'
import _ as dat from 'dat.gui'

// ...
자바스크립트
이제 Dat.GUI를 인스턴스화할 수 있습니다.

/\*\*

- Debug
  \*/
  const gui = new dat.GUI()
  자바스크립트
  /assets/lessons/10/step-02.png

그러면 화면 오른쪽 상단에 빈 패널이 생깁니다.

해당 패널에 추가할 수 있는 다양한 유형의 요소가 있습니다.

범위 - 최소값과 최대값이 있는 숫자의 경우
색상 - 다양한 형식의 색상용
텍스트 —간단한 텍스트의 경우
확인란 — 부울( true또는 false)
선택 - 값 목록에서 선택
버튼 - 기능 트리거
폴더 — 요소가 너무 많은 경우 패널을 구성합니다.
그 중 몇 가지를 살펴보겠습니다.

요소 추가
패널에 요소를 추가하려면 를 사용해야 합니다 gui.add(...). 첫 번째 매개변수는 개체이고 두 번째 매개변수는 조정하려는 해당 개체의 속성입니다. 관련 객체를 생성한 후 설정해야 합니다.

gui.add(mesh.position, 'y')
자바스크립트
/assets/lessons/10/step-03.png

패널에 범위가 표시되어야 합니다. 그것을 변경하고 큐브가 그에 따라 움직이는 것을 지켜보십시오.

최소값, 최대값 및 정밀도를 지정하려면 매개변수에서 설정할 수 있습니다.

gui.add(mesh.position, 'y', - 3, 3, 0.01)
자바스크립트
/assets/lessons/10/step-04.png

아니면 방법을 사용할 수 있습니다 min(...), max(...)그리고 step(...)사후이 직접 체인에 의한 add(...)방법 :

gui.add(mesh.position, 'y').min(- 3).max(3).step(0.01)
자바스크립트
한 줄에 너무 많은 메서드가 연결되어 있는 것이 싫다면 간단히 줄 바꿈을 추가할 수 있습니다.

gui
.add(mesh.position, 'y')
.min(- 3)
.max(3)
.step(0.01)
자바스크립트
레이블을 변경하려면 다음 name(...)방법을 사용하십시오 .

gui
.add(mesh.position, 'y')
.min(- 3)
.max(3)
.step(0.01)
.name('elevation')
자바스크립트
/assets/lessons/10/step-05.png

Dat.GUI는 조정하려는 속성의 종류를 자동으로 감지하고 해당 요소를 사용합니다. 좋은 예는 Object3D 의 visible속성입니다 . 이면 객체를 숨기는 부울입니다 .false

gui.add(mesh, 'visible')
자바스크립트
/assets/lessons/10/step-06.png

보시다시피 Dat.GUI는 visible속성이 boolean 이기 때문에 체크박스를 선택했습니다 .

wireframe재료 의 속성으로 동일한 작업을 수행할 수 있습니다 .

gui.add(material, 'wireframe')
자바스크립트
/assets/lessons/10/step-07.png

그림 물감
색상 처리가 조금 더 어렵습니다.

먼저 addColor(...)대신 를 사용해야 합니다 add(...). 이것은 Dat.GUI가 속성 유형만으로 텍스트, 숫자 또는 색상을 조정할지 여부를 알 수 없기 때문입니다.

둘째, 속성에 색상이 있는 중간 개체를 만들고 해당 속성을 재료에 사용해야 합니다. 이것은 Three.js 자료가 #ff0000.

parameter코드 시작 import부분 바로 뒤에 변수를 만듭니다 .

const parameters = {
color: 0xff0000
}
자바스크립트
그런 다음 gui변수를 인스턴스화한 후 다음 조정을 추가합니다.

gui.addColor(parameters, 'color')
자바스크립트
/assets/lessons/10/step-08.png

패널에 색상 선택기가 표시되어야 합니다. 문제는 이 색상을 변경해도 재질에 영향을 미치지 않는다는 것입니다. 그것은 변수 의 color속성을 변경 parameter하지만, 우리는 우리 재료에서 그 변수를 사용하지도 않습니다.

이 문제를 해결하려면 값이 변경될 때 알려주는 Dat.GUI가 필요합니다. onChange(...)메서드 를 연결하고 를 사용하여 재질 색상을 업데이트하면 됩니다 material.color.set(...). 이 방법은 때문에 당신처럼 사용할 수있는 방법을 다양한 형식의 매우 유용합니다 '#ff0000', '#f00', 0xff0000심지어 'red':

const parameters = {
color: 0xff0000
}

// ...

gui
.addColor(parameters, 'color')
.onChange(() =>
{
material.color.set(parameters.color)
})
자바스크립트
현재 0xff0000색상은 parameters개체 및 의 두 위치에서 지정됩니다 material.

큰 문제는 아니지만 색상을 변경하려면 이 두 곳에서 변경해야 합니다.

parameters.color머티리얼 의 속성 을 사용하여 간단히 수정해 보겠습니다 .

const material = new THREE.MeshBasicMaterial({ color: parameters.color })
자바스크립트
기능
색상 값과 같은 기능을 트리거하려면 해당 기능을 객체에 추가해야 합니다. parameters이전에 만든 객체 spin를 사용하여 큐브에 애니메이션 효과를 줄 함수가 포함 된 속성 을 추가 할 수 있습니다 .

const parameters = {
color: 0xff0000,
spin: () =>
{
gsap.to(mesh.rotation, { duration: 1, y: mesh.rotation.y + Math.PI \* 2 })
}
}
자바스크립트
그리고 다시 gui인스턴스화한 후 트윅을 추가할 수 있습니다 .

gui.add(parameters, 'spin')
자바스크립트
spin버튼 이 표시되어야 하며 이를 클릭하면 큐브가 360도 회전합니다.

팁
숨다
H패널을 숨기려면 누릅니다 .

패널을 처음부터 숨기려면 gui.hide()인스턴스화한 후 호출 하십시오.

닫기
패널의 하단 부분을 클릭하여 패널을 닫을 수 있습니다.

패널이 기본적으로 닫히도록 하려면 Dat.GUI를 인스턴스화할 때 개체를 보내고 closed: true속성에 전달할 수 있습니다.

const gui = new dat.GUI({ closed: true })
자바스크립트
너비
패널의 왼쪽 테두리를 끌어다 놓아 패널의 너비를 변경할 수 있습니다(항상 작동하는 것은 아니지만).

패널의 기본 너비를 변경 width: ...하려면 속성을 추가 합니다.

const gui = new dat.GUI({ width: 400 })
자바스크립트
Dat.GUI에 대해 더 알고 싶다면 다음 링크를 참조하십시오.

Github 저장소: https://github.com/dataarts/dat.gui
API 문서: https://github.com/dataarts/dat.gui/blob/HEAD/API.md
간단하지만 완전한 예: https://jsfiddle.net/ikatyang/182ztwao/
사용 방법 및 시기
다음 연습의 특정 순간에 디버그 패널을 사용할 것입니다. 그러나 원하는 만큼 조정을 자유롭게 추가할 수 있습니다. 이것은 연습을 하고 창의적인 것들을 만들기 시작하는 훌륭한 방법입니다.

진행하면서 조정을 추가하는 것이 좋습니다. 프로젝트 마지막에 모든 조정을 추가할 생각이라면 조정이 전혀 없는 상태로 끝날 것입니다.
