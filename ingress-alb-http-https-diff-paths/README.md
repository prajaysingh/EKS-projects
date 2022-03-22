This demo creates a ALB with following rules in place:

1) HTTP:80 Listener:

          Any request here on the host httpd.prajaysilwal.com and path (any) will be redirected to HTTPS:443 on host httpd.prajaysilwal.com and path index.html. Redirect to 2 a)
          
          
2) HTTPS:443 Listener:

      a) If the path is index.html and host is httpd.prajaysilwal.com, it will hit the backend service and pod
      
      b) If the path is httpd.prajaysilwal.com, it will redirect to HTTPS:443 on host httpd.prajaysilwal.com and path index.html. Redirect to 2 a)
