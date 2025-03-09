# 3D Quad Action Game
 ![메인](https://user-images.githubusercontent.com/74812194/156983608-eaa991c7-36c4-4fa2-b36f-9410464e06cb.gif)


플레이어가 이동하고 몬스터를 피격하고, 무기를 구입하고, 스테이지를 넘나드는 3D 쿼터뷰 액션 게임입니다.




# Version
Unity 2019.4.17f1




# Function
+ 플레이어 이동 (걷기, 점프, 회피, 무기 줍기&교체, 공격)
+ 해머, 핸드건, 머신건, 수류탄 공격 구현 (원거리, 근접공격)
+ 목표를 추적하는 AI 몬스터
+ 무기, 탄, 생명 상점 구현




# Code
코루틴(Coroutine)을 사용하여 Update 함수와 따로 일시적으로 돌아가는 서브 동작을 구현하거나, 어떤 다른 작업이 처리되는 것을 기다리는 기능을 구현을 하도록 하였습니다.
아래의 코드는 코루틴을 이용하여 A~C의 몬스터들이 시간차 공격을 하는 코드입니다.

``` C#
IEnumerator Attack()
    {
        isChase = false;
        isAttack = true;
        anim.SetBool("isAttack", true);

        switch (enemyType)
        {
            case Type.A:
                yield return new WaitForSeconds(0.2f);
                meleeArea.enabled = true;

                yield return new WaitForSeconds(1f);
                meleeArea.enabled = false;

                yield return new WaitForSeconds(1f);
                break;
            case Type.B:
                yield return new WaitForSeconds(0.1f);
                rigid.AddForce(transform.forward * 20, ForceMode.Impulse);
                meleeArea.enabled = true;

                yield return new WaitForSeconds(0.5f);
                rigid.velocity = Vector3.zero;
                meleeArea.enabled = false;

                yield return new WaitForSeconds(2f);
                break;
            case Type.C:
                yield return new WaitForSeconds(0.5f);
                GameObject instantBullet = Instantiate(bullet, transform.position, transform.rotation);
                Rigidbody rigidBullet = instantBullet.GetComponent<Rigidbody>();
                rigidBullet.velocity = transform.forward * 20;

                yield return new WaitForSeconds(2f);
                break;

        }

        isChase = true;
        isAttack = false;
        anim.SetBool("isAttack", false);
    }
```
