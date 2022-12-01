
# SARS-CoV-2-transmission-chain

## Main_code
```
% Matlab code
% bootstrap method
run=10000;
[bsp]=bootstrap_sample(A,run);
n=size(A,1);
bsp_protot_1=zeros(run,1);
for j=1:run
    bsp_protot_1(j,1)=sum(bsp(:,j))/n;
    for i=1:n
        if bsp(i,j)<2
            bsp_protot_1(j,1)=bsp_protot_1(j,1)+1/n;
        end
    end
end
bsp_protot_1=sort(bsp_protot_1);
bsp_pro_ci=[bsp_protot_1(floor(run*0.025+0.1),:);bsp_protot_1(floor(run*0.975+0.1),:)];

% bootstrap sample
function [bsp] = bootstrap_sample(A,run)
n=size(A,1);
seq=randi(n,n,run);
bsp=zeros(n,run);
for i=1:n
    for j=1:run
        bsp(i,j)=A(seq(i,j),1);
    end
end
end

% Negative binomial distribution through the maximum likelihood method
n=sum(X(:,1));
x_ave=X(:,1)'*X(:,2)/n; % mean value
s_2=(X(:,1)'*(X(:,2).*X(:,2))-n*x_ave^2)/(n-1); % var value
r_m=x_ave^2/(s_2-x_ave); % moment estimator
Z_m=n*log(r_m/(x_ave+r_m))+sum(X(:,1).*psi(X(:,2)+r_m))-n*psi(r_m);
r_1=0.005;
r_2=100;
epsilon=1e-3;
while abs(Z_m)>epsilon
    if Z_m>0
        r_1=r_m;
    else
        r_2=r_m;
    end
    r_m=(r_1+r_2)/2;
    Z_m=n*log(r_m/(x_ave+r_m))+sum(X(:,1).*psi(X(:,2)+r_m))-n*psi(r_m);
end
p_m=r_m/(x_ave+r_m);
col=[r_m,p_m];

```
