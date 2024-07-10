요즘 모던 자바 인 액션을 자기 전에 꾸준히 읽고 있는데 Makers에서 개발한 파트끼리의 랭킹 목록 조회 api를 스트림을 좀 더 적극적으로 활용하여 개선할 수 있을 것이라고 판단되어서 파트끼리의 랭킹 목록 조회 api를 리팩토링 해보려고 합니다.

이전 리팩토링 전 응답 속도입니다. 스레드 그룹은 아래와 같이 설정했어요
![[Pasted image 20240710104854.png]]

원래 Query
6명의 유저 and 유저 point를 가져옴
```
Hibernate: 
    select
        soptampuse0_.id as id1_10_,
        soptampuse0_.created_at as created_2_10_,
        soptampuse0_.updated_at as updated_3_10_,
        soptampuse0_.generation as generati4_10_,
        soptampuse0_.nickname as nickname5_10_,
        soptampuse0_.part as part6_10_,
        soptampuse0_.profile_message as profile_7_10_,
        soptampuse0_.total_points as total_po8_10_,
        soptampuse0_.user_id as user_id9_10_ 
    from
        app_dev.soptamp_user soptampuse0_ 
    where
        soptampuse0_.nickname like ? escape ?
Hibernate: 
    select
        soptamppoi0_.id as id1_9_,
        soptamppoi0_.created_at as created_2_9_,
        soptamppoi0_.updated_at as updated_3_9_,
        soptamppoi0_.generation as generati4_9_,
        soptamppoi0_.points as points5_9_,
        soptamppoi0_.soptamp_user_id as soptamp_6_9_ 
    from
        app_dev.soptamp_point soptamppoi0_ 
    where
        (
            soptamppoi0_.soptamp_user_id in (
                ?
            )
        ) 
        and soptamppoi0_.generation=?
Hibernate: 
    select
        soptampuse0_.id as id1_10_,
        soptampuse0_.created_at as created_2_10_,
        soptampuse0_.updated_at as updated_3_10_,
        soptampuse0_.generation as generati4_10_,
        soptampuse0_.nickname as nickname5_10_,
        soptampuse0_.part as part6_10_,
        soptampuse0_.profile_message as profile_7_10_,
        soptampuse0_.total_points as total_po8_10_,
        soptampuse0_.user_id as user_id9_10_ 
    from
        app_dev.soptamp_user soptampuse0_ 
    where
        soptampuse0_.nickname like ? escape ?
Hibernate: 
    select
        soptamppoi0_.id as id1_9_,
        soptamppoi0_.created_at as created_2_9_,
        soptamppoi0_.updated_at as updated_3_9_,
        soptamppoi0_.generation as generati4_9_,
        soptamppoi0_.points as points5_9_,
        soptamppoi0_.soptamp_user_id as soptamp_6_9_ 
    from
        app_dev.soptamp_point soptamppoi0_ 
    where
        (
            soptamppoi0_.soptamp_user_id in (
                ?
            )
        ) 
        and soptamppoi0_.generation=?
Hibernate: 
    select
        soptampuse0_.id as id1_10_,
        soptampuse0_.created_at as created_2_10_,
        soptampuse0_.updated_at as updated_3_10_,
        soptampuse0_.generation as generati4_10_,
        soptampuse0_.nickname as nickname5_10_,
        soptampuse0_.part as part6_10_,
        soptampuse0_.profile_message as profile_7_10_,
        soptampuse0_.total_points as total_po8_10_,
        soptampuse0_.user_id as user_id9_10_ 
    from
        app_dev.soptamp_user soptampuse0_ 
    where
        soptampuse0_.nickname like ? escape ?
Hibernate: 
    select
        soptamppoi0_.id as id1_9_,
        soptamppoi0_.created_at as created_2_9_,
        soptamppoi0_.updated_at as updated_3_9_,
        soptamppoi0_.generation as generati4_9_,
        soptamppoi0_.points as points5_9_,
        soptamppoi0_.soptamp_user_id as soptamp_6_9_ 
    from
        app_dev.soptamp_point soptamppoi0_ 
    where
        (
            soptamppoi0_.soptamp_user_id in (
                ? , ?
            )
        ) 
        and soptamppoi0_.generation=?
Hibernate: 
    select
        soptampuse0_.id as id1_10_,
        soptampuse0_.created_at as created_2_10_,
        soptampuse0_.updated_at as updated_3_10_,
        soptampuse0_.generation as generati4_10_,
        soptampuse0_.nickname as nickname5_10_,
        soptampuse0_.part as part6_10_,
        soptampuse0_.profile_message as profile_7_10_,
        soptampuse0_.total_points as total_po8_10_,
        soptampuse0_.user_id as user_id9_10_ 
    from
        app_dev.soptamp_user soptampuse0_ 
    where
        soptampuse0_.nickname like ? escape ?
Hibernate: 
    select
        soptamppoi0_.id as id1_9_,
        soptamppoi0_.created_at as created_2_9_,
        soptamppoi0_.updated_at as updated_3_9_,
        soptamppoi0_.generation as generati4_9_,
        soptamppoi0_.points as points5_9_,
        soptamppoi0_.soptamp_user_id as soptamp_6_9_ 
    from
        app_dev.soptamp_point soptamppoi0_ 
    where
        (
            soptamppoi0_.soptamp_user_id in (
                ? , ? , ?
            )
        ) 
        and soptamppoi0_.generation=?
Hibernate: 
    select
        soptampuse0_.id as id1_10_,
        soptampuse0_.created_at as created_2_10_,
        soptampuse0_.updated_at as updated_3_10_,
        soptampuse0_.generation as generati4_10_,
        soptampuse0_.nickname as nickname5_10_,
        soptampuse0_.part as part6_10_,
        soptampuse0_.profile_message as profile_7_10_,
        soptampuse0_.total_points as total_po8_10_,
        soptampuse0_.user_id as user_id9_10_ 
    from
        app_dev.soptamp_user soptampuse0_ 
    where
        soptampuse0_.nickname like ? escape ?
Hibernate: 
    select
        soptamppoi0_.id as id1_9_,
        soptamppoi0_.created_at as created_2_9_,
        soptamppoi0_.updated_at as updated_3_9_,
        soptamppoi0_.generation as generati4_9_,
        soptamppoi0_.points as points5_9_,
        soptamppoi0_.soptamp_user_id as soptamp_6_9_ 
    from
        app_dev.soptamp_point soptamppoi0_ 
    where
        (
            soptamppoi0_.soptamp_user_id in (
                ?
            )
        ) 
        and soptamppoi0_.generation=?
Hibernate: 
    select
        soptampuse0_.id as id1_10_,
        soptampuse0_.created_at as created_2_10_,
        soptampuse0_.updated_at as updated_3_10_,
        soptampuse0_.generation as generati4_10_,
        soptampuse0_.nickname as nickname5_10_,
        soptampuse0_.part as part6_10_,
        soptampuse0_.profile_message as profile_7_10_,
        soptampuse0_.total_points as total_po8_10_,
        soptampuse0_.user_id as user_id9_10_ 
    from
        app_dev.soptamp_user soptampuse0_ 
    where
        soptampuse0_.nickname like ? escape ?
Hibernate: 
    select
        soptamppoi0_.id as id1_9_,
        soptamppoi0_.created_at as created_2_9_,
        soptamppoi0_.updated_at as updated_3_9_,
        soptamppoi0_.generation as generati4_9_,
        soptamppoi0_.points as points5_9_,
        soptamppoi0_.soptamp_user_id as soptamp_6_9_ 
    from
        app_dev.soptamp_point soptamppoi0_ 
    where
        (
            soptamppoi0_.soptamp_user_id in (
                ? , ? , ? , ?
            )
        ) 
        and soptamppoi0_.generation=?
```