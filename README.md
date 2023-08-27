use ig_clone;
SELECT id,
       username,
       created_at
FROM   users
ORDER  BY created_at
LIMIT  6;

/* 
id	  username	       created_at

26    Orange_Splash     12-03-2017   00:54 

456   Cheesy_Nible      12-03-2017   00:54 

794   Love_Seeder       12-03-2017   00:54 

683   Peanut_Buzz       12-03-2017   00:54 

349   Improved_guys     12-03-2017   02:34 

986   Peter_parker      12-03-2017   02:34 

 */

SELECT u.id,
       u.username,
       Count(p.user_id) AS 'no._of_posts'
FROM   users u
       LEFT JOIN photos p
              ON u.id = p.user_id
GROUP  BY u.id
HAVING Count(p.user_id) = 0;

/*
 id	          username	                  no._of_posts
 
  23          Silent Eyes                      0 

  67          qwerty1                          0 

  34          Incomer Cozy                      0  

  67          honey_Bear                        0 

  789        Bee Grey                           0 

  765        sweetylyx                           0 

  324        Experienced Thoughts                0 

  678        Splash Elegant                     0 

  324        choexo                              0 

  78          Crunchy Crunch                     0 

  678        ferxanity                            0 

  26          eridanusly                          0 

  789        honey_bunny                         0 

  124        Mare Beloved                        0 

  324        Inspire You                        0 
*/

SELECT id,
       username
FROM   users
WHERE  id = (SELECT user_id
             FROM   photos
             WHERE  id = (SELECT photo_id
                          FROM   likes
                          GROUP  BY photo_id
                          ORDER  BY Count(photo_id) DESC
                          LIMIT  1)); 
						
/*
id	username
67	   Stalin_trantow

*/

SELECT t.tag_name,
       Count(t.tag_name) AS "tags count"
FROM   tags t
       INNER JOIN photo_tags ph
               ON t.id = ph.tag_id
GROUP  BY t.tag_name
ORDER  BY Count(t.tag_name) DESC
LIMIT  4; 


/*
tag_name	tags count

   fun     89 
   smile   78 
   reels   34 
   heart    56 


*/

SELECT     
DAYNAME(CREATED_AT) AS DAY,    
COUNT(DAYNAME(CREATED_AT)) AS USERS_REGISTERED
FROM    USERS
GROUP BY DAYNAME(CREATED_AT)
ORDER BY COUNT(DAYNAME(CREATED_AT)) DESC;

/*
   DAY	         USERS_REGISTERED

   Wednesday         49 
   Sunday            46 
   Friday            38 
   Monday            34 
   Saturday          31 
   Tuesday           29 
   Thursday          24 


*/

SELECT (SELECT COUNT(ID) FROM PHOTOS)/ (SELECT COUNT(DISTINCT USER_ID) FROM PHOTOS) AS Avg_posts_per_user,
(SELECT COUNT(ID) FROM PHOTOS)/ (SELECT COUNT(ID) FROM USERS) AS  Ratio;

/*
Avg_posts_per_user	Ratio
8.9876	2.3267
*/

SELECT id,
       username
FROM   users
WHERE  id IN (SELECT user_id
              FROM   likes
              GROUP  BY user_id
              HAVING Count(user_id) = (SELECT Count(id)
                                       FROM   photos)); 
                                       
/*
 id    user_name
67     qwerty1 
789    Bee Gray 
324    choexo 
26     eridanusly 
678    ferxanity 
124    Mare Beloved 
*/
